# Creative Director 小D

Creative Director 小D 是一個 Claude 技能，讓 AI 用世界級廣告公司的方法論扮演創意總監。你丟給它一包素材，它陪你把策略磨清楚，再發想創意、誠實打分，產出更好的行銷提案。

## 緣起

原作是一套英文的創意總監方法論技能（[smixs/creative-director-skill](https://github.com/smixs/creative-director-skill)）。作者在台灣行銷實務中使用時，需要的不只是翻譯，而是接上繁體中文工作流與真實提案節奏，於是 fork 逐步改造成這個版本。人格取 Creative Director 的字首 D 為名，暱稱「小D」。

## 怎麼召喚

對話中說「小D」「CD」「Creative Director」任一個即可，說「小D看案子」會直接進建檔流程。技能資料夾名稱維持 `creative-director`。

## 技能上下游

同作者的三個技能，可以在同一個對話裡接力：

| 技能 | 人格 | 負責 |
|---|---|---|
| creative-director（本技能） | 小D | 定策略、出創意概念 |
| [carousel-amy](https://github.com/uc94jooo/carousel-amy) | Amy | 說故事、做載體（排版、配圖、網頁） |
| [metacopy](https://github.com/uc94jooo/metacopy-tello) | 太羅 | 寫文案 |

交棒接口是版本化的策略對齊表明檔（`01_對齊表_[案名].md`），傳棒前有放行閘，由你點頭才交給下一棒。跨技能共通底線（反操控、防編造、防越權）定義在工作區 `AGENTS.md`。

## 幫你完成什麼

從雜亂素材到可提案的創意概念，整條路它都接手：

1. 建檔。檔案、貼上文字、網址都收，掃描後建立專案資料夾與主規劃檔。
2. 對齊策略。產出 13 點策略對齊表，資訊不足時主動提問，磨到你點頭通過才存檔。
3. 發想創意。每輪用三種以上不同路數的創意方法產出 8 至 12 個概念，先發散再收斂。
4. 誠實評估。多維度打分，分數不到門檻就換方法重改，最多五輪。
5. 交付與交棒。輸出提案格式的最終文件，並把定案策略傳給下游技能繼續加工。

## 解決你什麼痛點

用一般 AI 想創意，常見的下場是這幾種：

- 自由聯想式發想，量很大、質很平，而且常在不知情的狀況下抄到早已存在的名作。
- AI 天性愛奉承，自己打的分數一路灌水，你分不出哪個點子真的站得住。
- 策略還沒對齊就開始想創意，做到一半才發現方向錯，前面全部重來。
- 對話一長就失憶，換個 session 之前的推演全部歸零。

## 和其他類似技能有什麼不同

和一般 brainstorming 技能比：別人給你一串點子清單就結束，Creative Director 小D 堅持先找對洞察再想創意，每個點子都有分數、有改進方向、有跟真實名作的對照。

和上游原作比：原作是單線英文流程，本 fork 改成策略、創意兩段式，中間加上人工確認點，並接上繁中口語呼叫、互動式建檔、隱藏筆記系統。

和本工作區的其他創作技能比：Creative Director 小D 只管策略與創意概念，不寫成品。文案是太羅（metacopy）的事，社群輪播載體是 Amy（carousel-amy）的事，界線清楚，靠交棒單銜接。

## 為什麼能解決

方法論取代自由聯想。內建 20 多種結構化創意方法，每一輪發想強制混用不同路數，不會同一招想到底。

點子新不新，比對真實案例，不是 AI 自己說了算。內建經典廣告案例庫（上游原有 571 篇，本 fork 持續擴充與淘汰中，現況以 `references/legendary-campaigns/MOC-index.md` 統計為準），每個點子都拿去跟史上名作比對，同樣的手法已經被用過很多次，分數就高不起來，老梗混不進來。

評分不灌水。拿真實得獎作品當標準，好壞照實講，還會反過來攻擊自己最喜歡的點子，測試它到底站不站得住。

流程有人工閘門。策略沒經你確認通過，不會開始發想；發想與交棒前也都要你點頭，AI 不會自作主張往前衝。

推演有留痕。每個專案的 `.cd-notes/` 隱藏資料夾記錄推演過程與對話總帳，換個 session 也能接著做，同一張對齊表能支撐多輪發想。

## Fork 來源與致謝

原作 [smixs/creative-director-skill](https://github.com/smixs/creative-director-skill)，作者 Sergey Shima，與 Paul Deadcough 協作，MIT 授權。方法論核心（20+ 創意方法、六權重評分、Cannes/HumanKind 校準、案例庫、P01-P18 模式地圖）全數保留，本 fork 的完整改動史見 [changelog.md](changelog.md)。

方法論源頭：Jacob Goldenberg（SIT）、Genrich Altshuller（TRIZ）、Edward de Bono（水平思考）、Arthur Koestler（Bisociation）、Leo Burnett（HumanKind）、Mark Pollard（七層級創意分類）、Clayton Christensen（JTBD）等。案例庫取材自 D&AD、Cannes Lions、One Show、Effie 等獎項名錄。

## 安裝與執行

```bash
git clone https://github.com/uc94jooo/creative-director.git ~/.claude/skills/creative-director
```

本 fork 已扁平化，`SKILL.md` 就在 repo 根目錄，clone 完重啟 Claude Code session 即生效。

## 不適用範圍

媒體採購與預算分配、製作管理、品牌識別設計、成品文案（交給太羅）、市場調查數據收集。

## 授權

MIT。原作 Copyright (c) 2026 Sergey Shima；本 fork 修改 Copyright (c) 2026 Johs (uc94jooo)。詳見 [LICENSE](LICENSE)。
