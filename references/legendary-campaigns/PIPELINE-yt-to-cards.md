# Pipeline：YouTube 案例影片 → legendary-campaigns card

> 適用場合：坎城、D&AD、One Show 等創意獎得獎案例的 case study 影片，轉換成可供技能查閱的結構化 card。

> **註記（2026-06-24 by johs）：** 本 pipeline 的「輸入端」（yt-dlp 抓字幕）僅適用於 YouTube case study 影片。
> landing page 等網頁類標竿不適用此輸入端，需改用 `webpage-capture` skill 擷取頁面文字後，再進入步驟 3（生成 Card）。後段卡片 schema 與 MOC 索引完全沿用。

---

## 概覽

```
YouTube 播放清單
    ↓ yt-dlp 下載字幕 (.vtt)
scripts/yt-cases/subtitles/
    ↓ 清洗 VTT（去重複行、去時間碼、去 <c> tag）
純文字逐字稿
    ↓ Claude（本 session 直接生成，或 vtt_to_cards.py + API key）
legendary-campaigns/cards/{id}.md
    ↓ scripts/generate_mocs.py
MOC-index.md / MOC-pattern.md / … 更新
    ↓ 更新 SKILL.md + README.md 數字
完成
```

---

## 步驟說明

### 1. 下載字幕

```bash
cd /Users/johs-ai/Desktop/AI-Project/marketing-rag/scripts/yt-cases

/Users/johs-ai/Desktop/AI-Project/marketing-rag/venv/bin/yt-dlp \
  --write-auto-sub --sub-lang en --skip-download \
  --output "subtitles/%(title)s.%(ext)s" \
  "https://www.youtube.com/playlist?list=PLAYLIST_ID"
```

- 下載位置：`subtitles/`
- 格式：`.en.vtt`（自動字幕）
- 部分影片無字幕或地區限制，會跳過（不影響其他檔案）

**安裝 yt-dlp（已在 venv）：**
```bash
/Users/johs-ai/Desktop/AI-Project/marketing-rag/venv/bin/pip install yt-dlp
```

### 2. 清洗 VTT

VTT 原始格式包含重複行、時間碼行、`<c>` inline tag。清洗邏輯：

```python
import re

def clean_vtt(text):
    prev = None
    out = []
    for line in text.splitlines():
        line = line.strip()
        if not line or line == ' ': continue
        if line.startswith('WEBVTT') or line.startswith('Kind:') or line.startswith('Language:'): continue
        if re.match(r'^\d{2}:\d{2}:\d{2}\.\d{3} -->', line): continue
        line = re.sub(r'<\d{2}:\d{2}:\d{2}\.\d{3}>', '', line)
        line = re.sub(r'</?c>', '', line).strip()
        if not line or line == ' ': continue
        if line != prev:
            out.append(line)
            prev = line
    return ' '.join(out)
```

### 3. 生成 Card

**方法 A：Claude session 直接生成（無需 API key）**

在 Claude Code session 中，讓 Claude 讀取清洗後的逐字稿，按 17-axis schema 手動撰寫 card。適合一次性處理、品質最高。

**方法 B：批次腳本（需 ANTHROPIC_API_KEY）**

```bash
export ANTHROPIC_API_KEY=sk-ant-...
/Users/johs-ai/Desktop/AI-Project/marketing-rag/venv/bin/python3 \
  scripts/yt-cases/vtt_to_cards.py
```

腳本位置：`scripts/yt-cases/vtt_to_cards.py`
- 自動讀取 `subtitles/*.vtt`
- 清洗後呼叫 `claude-opus-4-8`（含 adaptive thinking）
- 輸出至 `legendary-campaigns/cards/{id}.md`
- 已存在的 card 不會覆蓋（需手動確認）

### 4. 更新 MOC 索引

```bash
/Users/johs-ai/Desktop/AI-Project/marketing-rag/venv/bin/python3 \
  .claude/skills/creative-director/scripts/generate_mocs.py
```

輸出訊息格式：`Generated 6 MOCs from {N} cards (0 errors skipped)`

### 5. 更新數字

新 card 加入後，以下三個位置的數字需同步：

| 檔案 | 需更新位置 |
|------|-----------|
| `SKILL.md` | `library of {N} legendary campaigns`（2 處）|
| `README.md` | 第 3 行、目錄結構說明（共 3 處）|

---

## Card schema（17 axes）

詳見 `[[../tag-schema.md]]`。必填欄位：

```yaml
id: {brand-slug}-{title-slug}-{year}
title, brand, agency, year, country, region, industry
pattern: [P01]          # P01–P18
category                # film|integrated|stunt_pr|digital_product|ooh_print|social|experiential|data_driven
idea_type               # campaign|execution|stunt|platform
involvement             # passive_view|lean_forward|participation|co_creation
channel, duration
goal: [awareness]
budget: low|medium|high
emotion: [slug]
emotion_tier: 1|2|3
insight_domain
media_epoch
awards: [cannes_grand_prix]
quality_score: 1-10
scalability, risk
confidence: high|medium|low
quality: full           # 從逐字稿生成，一律 full
source_url              # 留空或填 YT 連結
```

五個 body sections：**Insight / Mechanic / Why it worked / Steal / Related**

---

## 實際執行紀錄

| 日期 | 來源 | 新增 cards | 總數 |
|------|------|-----------|------|
| 2026-06-24 | 2024 Cannes Lions Grand Prix 播放清單（19 支 VTT） | 14（5 支原有） | 585 |

---

## 已知問題

- **5 支無字幕影片**（2024 批次）：Hornbach、Sol Cement、Siemens Healthineers、Mastercard、Vaseline。可能原因：無自動字幕、地區限制。備選方案：下載音訊後用 Whisper 轉錄（`~/.cache/whisper/medium.pt` 已在本機）。
- **逐字稿品質差異**：自動字幕對人名、品牌名誤轉率高，card 生成後需人工校對專有名詞。
- **source_url 未驗證**：腳本填入播放清單 URL 而非各影片個別連結，需手動補正。
