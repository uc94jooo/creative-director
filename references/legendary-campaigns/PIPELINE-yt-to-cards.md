# Pipeline：YouTube 案例影片 → legendary-campaigns card

> 適用場合：坎城、D&AD、One Show 等創意獎得獎案例的 case study 影片，轉換成可供技能查閱的結構化 card。

> **註記（2026-06-24 by johs）：** 本 pipeline 的「輸入端」（yt-dlp 抓字幕）僅適用於 YouTube case study 影片。
> landing page 等網頁類標竿不適用此輸入端，需改用 `webpage-capture` skill 擷取頁面文字後，再進入步驟 3（生成 Card）。後段卡片 schema 與 MOC 索引完全沿用。

---

## 概覽

```
YouTube 播放清單
    ↓ yt-dlp --flat-playlist 只抓「標題清單」（不下載）
標題清單
    ↓ 去重比對關卡（對庫裡 cards/ 做品牌＋案例名模糊比對）
確定新案例名單
    ↓ yt-dlp 下載字幕 (.vtt) ＋ 說明文字 (.description)
scripts/yt-cases/subtitles/
    ↓ 清洗 VTT（去重複行、去時間碼、去 <c> tag）
純文字逐字稿（＋說明文字當 metadata 校正來源）
    ↓ Claude（本 session 直接生成，或 vtt_to_cards.py + API key）
legendary-campaigns/cards/{id}.md
    ↓ creative-director/scripts/generate_mocs.py
MOC-index.md / MOC-pattern.md / … 更新
    ↓ 更新 SKILL.md + README.md 數字
    ↓ 在本文件「實際執行紀錄」表加一行（正式步驟，不可省略）
完成
```

---

## 步驟說明

### 0. 抓標題清單＋去重比對關卡（生卡前必做）

先只抓標題，一支影片都不下載：

```bash
/Users/johs-ai/Desktop/Claude/mkt-rag/venv/bin/yt-dlp \
  --flat-playlist --print "%(playlist_index)s|%(id)s|%(title)s" \
  "https://www.youtube.com/playlist?list=PLAYLIST_ID"
```

⚠️ **抓到的筆數必須與播放清單實際數量核對**。舊版 yt-dlp 抓 YouTube 續頁會靜默失敗，
剛好停在 100 筆（2026-08-06 實際發生：189 筆只抓到 100）。筆數不符先升級：
`mkt-rag/venv/bin/pip install -U yt-dlp`。

比對規則（由 Claude 在 session 內做，不靠腳本）：

- 拿標題清單對 `cards/` 全部檔名＋ MOC-index，用**品牌＋案例名**模糊比對。
  不可只靠 slug 精確比對——YouTube 標題生出的 slug 常跟庫內既有卡對不上
  （庫裡已因此出過 dramamine、pop-tarts、spotify-spreadbeats、pedigree-adoptable 四組重複卡）。
- **年份不作為去重判斷依據**（campaign 年、得獎年、上傳年三者常不一致）。
- 分類為：確定新案例／確定重複／疑似重複。疑似者由 Claude 推測裁決並附理由
  （使用者已授權：無法人工比對，由 Claude 判定）。
- 撞到重複時檢查舊卡品質：若舊卡是低品質來源（非 full、無逐字稿），
  而本批有完整 case study 逐字稿，選「升級改寫舊卡」而非跳過。
- 回顧型影片（retrospective，內容為既有卡案例的總回顧）直接跳過，不建卡。

### 1. 下載字幕＋說明文字（只下載「確定新案例」名單）

```bash
cd /Users/johs-ai/Desktop/Claude/mkt-rag/scripts/yt-cases

/Users/johs-ai/Desktop/Claude/mkt-rag/venv/bin/yt-dlp \
  --write-auto-sub --sub-lang en --write-description --skip-download \
  --output "subtitles/batch-YYYY-MM-DD/%(title)s.%(ext)s" \
  -a 新案例URL清單.txt
```

- 每批建獨立子資料夾 `batch-YYYY-MM-DD/`，不與舊批混放
- 格式：`.en.vtt`（自動字幕）＋ `.description`（說明文字）
- **說明文字必抓**：案例影片說明欄常含 campaign 正式名稱、品牌、代理商、年份、
  得獎紀錄與案例簡介，是 frontmatter 的校正來源（自動字幕對專有名詞誤轉率高）。
  無字幕的影片若說明文字夠詳細，可直接從說明文字生低 confidence 卡，不必動用 Whisper。
- 部分影片無字幕或地區限制，會跳過（不影響其他檔案）

**無字幕／逐字稿為雜訊的項目，必須另存永久來源紀錄**（2026-08-06 使用者定案）：
在 `legendary-campaigns/SOURCES-log-{日期}-no-subtitle.md` 記下案例名、卡片 id、
影片網址、完整說明文字全文。這些卡片的唯一佐證是說明文字，而 `subtitles/` 底下
的批次資料夾屬工作暫存，不應是低信度卡的唯一依據來源；沒有這份紀錄，日後要
覆核這些卡片的判斷依據就無從查起。範例：`SOURCES-log-2026-08-06-no-subtitle.md`。

**安裝 yt-dlp（已在 mkt-rag venv）：**
```bash
/Users/johs-ai/Desktop/Claude/mkt-rag/venv/bin/pip install yt-dlp
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

**年份規則（2026-08-06 使用者定案）**：campaign 年與得獎年不一致時，一律取**較早**者，
錯了沒關係。來源優先序：說明文字 > 逐字稿內文 > YouTube 上傳日期（最後手段）。

**方法 B：批次腳本（需 ANTHROPIC_API_KEY）**

```bash
export ANTHROPIC_API_KEY=sk-ant-...
/Users/johs-ai/Desktop/Claude/mkt-rag/venv/bin/python3 \
  scripts/yt-cases/vtt_to_cards.py
```

腳本位置：`mkt-rag/scripts/yt-cases/vtt_to_cards.py`
- 自動讀取 `subtitles/*.vtt`
- 清洗後呼叫 `claude-opus-4-8`（含 adaptive thinking）
- 輸出至 `legendary-campaigns/cards/{id}.md`
- 已存在的 card 不會覆蓋（需手動確認）

### 4. 更新 MOC 索引

```bash
/Users/johs-ai/Desktop/Claude/mkt-rag/venv/bin/python3 \
  /Users/johs-ai/Desktop/Claude/.claude/skills/creative-director/scripts/generate_mocs.py
```

輸出訊息格式：`Generated 6 MOCs from {N} cards (0 errors skipped)`

### 5. 更新數字

新 card 加入後，以下三個位置的數字需同步：

| 檔案 | 需更新位置 |
|------|-----------|
| `SKILL.md` | `library of {N} legendary campaigns`（2 處）|
| `README.md` | 第 3 行、目錄結構說明（共 3 處）|

### 6. 記錄本次執行（正式步驟，不可省略）

在本文件底部「實際執行紀錄」表格加一行，欄位：日期、來源、清單筆數、
新增 cards、重複（跳過）、無字幕／失敗、總數。沒有這行，下一批進料就無帳可對。

---

## Card schema（17 axes）

詳見 `[[../tag-schema.md]]`。必填欄位：

```yaml
id: {brand-slug}-{title-slug}-{year}
title, brand, agency, year, country, region, industry
pattern: [P01]          # P01–P18
category                # film|integrated|stunt_pr|digital_product|ooh_print|social|experiential|data_driven
idea_type               # campaign|execution|stunt|platform
involvement             # passive_view|lean_forward|participation|user_co_author
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

| 日期 | 來源 | 清單筆數 | 新增 cards | 重複（跳過） | 無字幕／失敗 | 總數 |
|------|------|---------|-----------|-------------|-------------|------|
| 2026-06-24 | 2024 Cannes Lions Grand Prix 播放清單 | 19 支 VTT | 14 | 5 | 5（Hornbach 等，見已知問題） | 585 |
| 2026-08-06 | Cannes Case Study 播放清單（189 支，`PLpmnJLueKdA6qwOfP74Zvl6B_H2knMVF6`） | 189 支標題 | 125 | 64（步驟 0 篩出 39 確定重複＋4 疑似裁決＋9 步驟 0 漏檢但生卡時被 id 查重擋下） | 20（無字幕，改用說明文字生 stub_enriched 卡；其中 3 支逐字稿為純音樂雜訊亦同樣處理） | 716 |

---

## 已知問題

- **5 支無字幕影片**（2024 批次）：Hornbach、Sol Cement、Siemens Healthineers、Mastercard、Vaseline。可能原因：無自動字幕、地區限制。備選方案：下載音訊後用 Whisper 轉錄（`~/.cache/whisper/medium.pt` 已在本機）。
- **逐字稿品質差異**：自動字幕對人名、品牌名誤轉率高，card 生成後需人工校對專有名詞。
- **source_url 未驗證**：腳本填入播放清單 URL 而非各影片個別連結，需手動補正。
- **人工比對規模有上限**（2026-08-06）：189 支的清單只有前 100 支經過人工逐一核對去重，
  後 89 支未核對就直接送去生卡，結果 9 支撞上既有卡（生卡 agent 的 id 查重機制擋下，
  未造成重複卡）。清單超過一定量時，步驟 0 的比對應改由 Claude 對全部清單做完整比對，
  不可只做部分抽查；生卡階段的 id 查重是最後防線，不是可省略的第一道關卡。
- **schema 與既有卡片有落差**（2026-08-06 發現）：`validate_schema.py` 顯示庫內有 15 張舊卡
  （多為 2024 坎城批次，含曾被當作黃金範例的 `pedigree-adoptable-2024`）使用了不在
  `tag-schema.md` 枚舉表內的值（如 `fmcg_food`、`lean_forward`、`social_change`、`ai_era`、
  `platform` 等）。本次新增的 125 張全部依 tag-schema.md 現行枚舉填寫，未沿用這些值；
  15 張舊卡是否要改值符合 schema、或 schema 該擴充納入這些值，待使用者裁決，尚未處理。
