# Creative Director (小D) 版本變更日誌 (Changelog)

本文件紀錄 `creative-director` (小D) 技能的安裝、功能迭代、案例庫更新及規則調教歷史，確保技能演進有跡可循。

---

## 📖 變更歷史

### 🟢 v2.0.4 (2026-07-09)
* **修改時間**：2026-07-09 (Local Time)
* **說明**：
  * **產線工件編號命名**：`segment-strategy.md` 對齊表存檔改為 `01_對齊表_[案名].md`,重跑版本改用檔名尾碼(`_v2`)。編號固定=語意插槽(01對齊表→02講稿→03交棒單→04文案包→05輪播),日期移至 frontmatter。適用新案,舊專案不回溯改名。編號總表見全域 AGENTS.md 第四節。

### 🟢 v2.0.3 (2026-07-09)
* **修改時間**：2026-07-09 (Local Time)
* **說明**：
  * **全域 AGENTS.md 建立與接線**：v2.0.2 的替身(Alias)解析依賴工作區 `AGENTS.md`,但該檔此前從未建立。現已於工作區根目錄建立,含「📂 外部路徑對照與替身規則」章節(目前為空表,待使用者登記),以及產線三技能(小D/Amy/太羅)共用底線:反操控文案嚴禁清單、防資訊編造、防越權、動態存檔路徑。
  * **SKILL.md 加指向**:Anti-Pitfall Rules 段落開頭新增一行,指明跨技能共通底線見全域 `AGENTS.md`,以下僅列本技能特有規則。

### 🟢 v2.0.2 (2026-07-08)
* **修改時間**：2026-07-08 (Local Time)
* **說明**：
  * **Phase 0 解耦與重構**：將 Ingestion (Phase 0 工作區路徑與防呆掃描、替身解析、檔案多選) 相關流程從 `references/segment-strategy.md` 中徹底解耦，移入專屬的 `references/segment-ingestion.md` (原 `phase0-build-standard.md` 重新命名)，使 `segment-strategy.md` 專注於 Phase 1 & 2 的策略推演。
  * **Alias 動態對照支援**：在 `segment-ingestion.md` 中實作動態 alias 解析規則，使小D 讀取工作區的 `AGENTS.md` 外部路徑對照，移除技能檔中寫死的個人絕對路徑，提升跨平台通用性。
  * **影片素材前處理接線**：`segment-ingestion.md` 新增「1.5 影片素材前處理」，本機影片檔案輸入時自動轉呼叫共用的 `video-transcript` 技能，取得逐字稿與關鍵幀讀圖後才進入標準建檔流程。

### 🟢 v2.0.1 (2026-07-08)
* **修改時間**：2026-07-08 (Local Time)
* **說明**：
  * **目錄結構扁平化**：移除多餘嵌套的 `creative-director` 子目錄，將 `SKILL.md`、`changelog.md` 和 `references/` 直接提升至專案根目錄下，徹底優化 RAG 增量索引與 Agent 沙盒環境下的文件讀取效率。

### 🟢 v2.0.0 (2026-07-07)
* **修改時間**：2026-07-07 (Local Time)
* **說明**：
  * **實作 Checkpoint 2 (放行點 / 傳球門檻)**：於 `SKILL.md` 的十步驟骨架中正式實作第 11 步（Checkpoint 2），並在 `segment-strategy.md` 中新增「人工介入點：放行」規則——小D 存檔策略對齊表並寫入 `dialogue-log` 後，必須終止 Turn 暫停，明確取得人類的「放行/傳球」同意。
  * **Git 追蹤清理與目錄重構**：將 `johs-notes/` 加入 `.gitignore` 排除追蹤，並在 Git 倉庫中刪除該目錄的追蹤（本地保留）。建立 `skill-workflow/` 目錄並搬移安裝說明文件至此，重新加入 Git 追蹤。
  * **技能流程圖重構**：新增與 AMY 形式一致的 `skill-workflow/` 流程可視化網頁及 Markdown 說明文件，完整呈現策略段與創意段的十步驟流程與 Checkpoint 機制。

### 🟢 v1.9.2 (2026-07-05)
* **修改時間**：2026-07-05 (Local Time)
* **說明**：
  * **召喚時版本檢查**：SKILL.md 新增「Version Check on Summon」常駐節——每 session 首次召喚小D 時，抓取 GitHub 遠端 changelog 比對版本號；遠端較新則摘述新版變更並詢問是否更新（同意→複製覆蓋本地並提醒下個 session 才完整生效；拒絕→本 session 不再問）；抓取失敗不阻塞。

### 🟢 v1.9.1 (2026-07-05)
* **修改時間**：2026-07-05 (Local Time)
* **說明**：
  * **人工介入點統一命名**：廢除易混淆的 Checkpoint 1／Checkpoint 2 編號制，統稱「人工介入點」，以功能命名實例：「人工介入點：策略確認」（簡稱策略確認點，原 Checkpoint 1）、「人工介入點：放行」（簡稱放行點，原 Checkpoint 2，未實作）。SKILL.md、segment-strategy.md、README 同步改名；changelog 歷史條目維持當時用語（v1.2 除外，該條同時做了未實作項清理）。
  * **changelog 只留實作事實**：v1.2 條目中從未實作的「白話行銷術語表」與「放行點（Amy 交棒授權）」自 changelog 移除，轉入 `wishlist.md` 待辦。
  * **wishlist 擴充**：新增第 3-5 項（策略確認點呈現細緻度分級／放行點／白話行銷術語表），並建立與 `johs-notes/使用者判斷-2026-0702.md` 的關聯註記。

### 🟢 v1.9 (2026-07-05)
* **修改時間**：2026-07-05 (Local Time)
* **設計原則**：SKILL.md 求的不是「檔案最短」而是「常駐成本最小、行為閘門不外移」。判準：某段拿掉後，模型會不會在讀到 reference 之前就犯錯——會則留、不會則外移。
* **說明**：
  * **兩段式架構**：依「策略段（Phase 0-2，產出對齊表即結案）／創意段（Phase 3-6，生成與檢驗）」拆分。SKILL.md 瘦身為常駐骨架（652 → 約 260 行），Phase 全文外移至兩個段落包：
    * `references/segment-strategy.md`（Phase 0/1/2 全文）
    * `references/segment-creative.md`（Phase 3/4/5/6 全文）
  * **段落邊界規則**：策略段結案物＝三件套（對齊表 vX＋insight-pass 串＋dialogue-log）；結案後停住，嚴禁自動滑入 Phase 3。創意段進場檢查：偵測既有 `*_策略對齊表*.md` → 載入直接開工不重跑策略段（支援跨 session 斷點續作、一表多輪發想）。
  * **常駐硬閘門清單**：六條閘門（掃描代選閘、診斷閘、Checkpoint 閘、結案閘、HumanKind<7 輸出閘、段落包必讀閘）獨立成節留在常駐區，防 v1.6「閘門隨細節外移而失效」的翻車重演。
  * **尾段去重瘦身**：Creative Constitution 短版、HumanKind 量表、Calibration 雙系統、Output Format 四節自 SKILL.md 移除（`scoring-calibration.md` 與 `output-templates.md` 已有完整版）；Gap Analysis 四行表原 references 缺漏，補入 `scoring-calibration.md` §8。
  * 十步驟骨架、Hidden Notes 整章、Anti-Pitfall Rules 維持常駐不動。

### 🟢 v1.8 (2026-07-05)
* **修改時間**：2026-07-05 (Local Time)
* **背景**：本地工作機（marketing-rag）與 GitHub 兩線分岔——本地 7/3 加入 Hidden Notes 系統但未推送，remote 7/5 完成 v1.7 Phase 0 重構。本版以 remote v1.7 為底合併兩線，並補入 Mia C'bon 2026 案實跑（Phase 0→Checkpoint 1 三輪深挖）驗證出的新規則。
* **說明**：
  * **移植回 Hidden Notes 整章**（v1.7 遺失的本地功能）：`.cd-notes/` 隱藏資料夾、線頭題庫 (Breadcrumb Bank)、新手／老手訊號判定。
  * **新增「標準週期流程（十步驟骨架）」**：新案從進場到 Checkpoint 1 通過的固定節奏總覽表，明確每步產出物與兩種隱藏檔的寫入時機。
  * **insight-pass 寫入規則條文化**（Hidden Notes A）：三觸發條件（引擎跑完／Phase 4 recursion／使用者修正致對齊表改寫）、機械化編號規則、同輪追加 vs 跨輪新檔之區分。
  * **新增 dialogue-log 機制**（Hidden Notes A-2）：週期結算式對話紀錄檔，固定四欄結構＋沉澱區。
  * **新增寫入判準對照表**（Hidden Notes A-3）：insight-pass 跟「表的變化」走、dialogue-log 跟「週期」走。
  * **Checkpoint 1 擴充**：使用者深挖/修正時的處理循環（收斂規則＋禁止事項）、對齊表落地存檔步驟（固定檔名格式＋frontmatter 版本欄＋不覆寫舊版）。
  * **細項移植**：Phase Router 中文口語呼叫規則、對齊表排版規則（強制分點條列）、Phase 0 `_Project/` 防混淆警告。
  * 所有新條文以低解讀力模型為適配基準撰寫（機械判斷條件、編號步驟、模板、禁止清單）。

### 🟢 v1.7 (2026-07-05)
* **修改時間**：2026-07-05 12:45 (Local Time)
* **說明**：
  * **Phase 0 Ingestion 流程防呆與結構重構**：更新 `SKILL.md` 與 `references/phase0-build-standard.md`，將 Phase 0 改良為動態相對路徑掃描，加入防通靈與防代選設計。
  * **檔案庫清理與規劃**：刪除 `references/security-lock-draft.md`，建立 `wishlist.md` 來記錄未來的檔案命名規範優化與安全機制，並重命名變更記錄與思考文件。

### 🟢 v1.6 (2026-07-03)
* **修改時間**：2026-07-03 00:18 (Local Time)
* **說明**：
  * **Phase 0 Ingestion 流程模組化**：將 Phase 0 的互動式建檔與 Ingestion 詳細流程從總指揮官 `SKILL.md` 中抽出，移入獨立的策略參考文件 `[[references/phase0-build-standard.md]]`，並在 `SKILL.md` 中加入實體載入指令。此舉解決了 `SKILL.md` 冗長且因包含無效編碼字符（在 Phase 0 重複寫入的瑕疵段落中）導致工具讀取失敗的痛點。

### 🟢 v1.5 (2026-07-02)
* **修改時間**：2026-07-02 23:59 (Local Time)
* **說明**：
  * **策略對齊表受眾欄位雙軌化**：正式修改 `SKILL.md` 中對齊表第 7 點的規範。強制受眾分析必須以「雙軌對照結構」呈現，即完整列出客戶原始 Brief 受眾的人口學數據（年齡、人數、Index、標籤），再並列轉譯出小D 的行為人格與生活微場景，防止創意定位與市調預算邊界脫節。

### 🟢 v1.4 (2026-07-02)
* **修改時間**：2026-07-02 23:38 (Local Time)
* **說明**：
  * **模組化策略架構重構**：為了確保 Skill 在不同 AI 助理與平台間的移植性與執行意志，將「一主二副品牌原型」、「受眾角色鑑定法」、「空雨傘推演」與「四大思維避坑防線」從總指揮官 `SKILL.md` 中抽離，移至獨立的策略參考文件 `[[references/insight-mining-private-strategy-johs.md]]`。
  * **主控權釋放**：`SKILL.md` 現在保持極度精簡，專注於流程總控與調用宣告，降低大腦負載。
  * **強制性加載**：在 `SKILL.md` 中明確寫入載入指令，強制 AI 必須於 Phase 2 開頭實體呼叫並載入 `references/insight-mining.md` 與 `references/insight-mining-private-strategy-johs.md`。

### 🟢 v1.3 (2026-07-02)
* **修改時間**：2026-07-02 21:24 (Local Time)
* **說明**：
  * **建立「專案資料區」**：在根目錄建立並初始化「[專案資料區](專案資料區)」資料夾。
  * **實作 Phase 0: INGESTION (原始資料導入與建檔)**：小D 開始支援自動檢測專案資料區子目錄檔案或對話中 Google Docs 網址，並將其自動轉檔歸檔為標準 MD 檔案。
  * **重塑 Phase 1: INTAKE (需求解讀與限制判定)**：
    * 整合 13 點策略對齊之前 6 重點（限制與戰場、品牌與承諾）萃取規則。
    * 導入【競品對標攻擊準則】以分析對手體感與使用缺陷。
    * 擴充「資訊充足度診斷」提問機制。
    * 整合並中文化 Pollard 7層創意層級與 3大活動體驗防雷診斷測試。
  * **重塑 Phase 2: INSIGHT (深度洞察與對齊表生成)**：
    * 實作底層 **【隱性背景策略推演引擎】**，整合「品牌十二原型（一主二副）」、「受眾角色鑑定法（他是誰、怕什麼、不怕什麼）」與「麥肯錫空雨傘法」。
    * **擴充受眾分析底層邏輯**：正式導入【避免受眾分析的四大思維陷阱 (Anti-Trap Safeguards)】，強制 AI 在思考受眾時防範「二分法對抗與刻板印象」、「混淆格式與決策過濾」、「漏失中間過渡客群」及「使用虛無行銷詞彙」，強制將受眾痛點與行為還原為具體的生活微場景。
    * 增設【隱形輸出規則】，嚴禁在輸出文本中直接顯露上述方法論名稱。
    * 實施 Checkpoint 1 強制暫停機制。
  * **防坑指南更新**：新增第 10 條「真實性與溝通誠實」規則，嚴禁 AI 進行『假裝正在後台運算』等虛偽角色扮演對話包裝，要求直白、誠實地向使用者說明目前的執行狀態。

### 🟢 v1.2 (2026-07-01)
* **修改時間**：2026-07-01 22:39 (Local Time)
* **說明**：
  * 規劃與對焦小D的「技能調教 (Refinement)」，目標是提升跨案通用的行銷策略與協作體驗。
  * **核心調教項目（提案中，後續已實作者）**：
    1. **語系中文化**：以繁體中文重新編寫核心階段。
    2. **重塑 Phase 1 & 2**：強制小D 先輸出「策略對齊表」通用模板（含 4 大板塊與 13 個重點），確認後才進行創意發想。
    3. **受眾與競品分析準則優化**：
       * 受眾：拒絕年齡二分法，改依據購買決策、避雷心理等行為細分。
       * 競品：主動剖析競品「使用細節（如黏膩、起屑）」與「定位破綻」作為我方主動攻擊點。
    4. **人工把關閘門 (Human-in-the-loop)**：
       * **策略確認點**（時稱 Checkpoint 1）：輸出對齊表後強制暫停，等待確認後才發想。
  * 註：本版原提案中的未實作項目（白話行銷術語表、放行點——傳遞策略給 Amy 前人工授權，時稱 Checkpoint 2）已移至 `wishlist.md` 待辦清單。

### 🟡 v1.1
* **修改時間**：2026-06-24 ~ 2026-06-26
* **說明**：
  * **案例庫擴充**：下載 2024 Cannes Lions Grand Prix 播放清單（自動字幕檔），新增 14 個結構化案例卡片，使案例總數由 571 增至 **585** 個。
  * **索引重編**：重跑 `generate_mocs.py` 重新整理 6 個 MOC 分類文件與 `README.md`。
  * **Pipeline 補強 (06-26)**：在 `PIPELINE-yt-to-cards.md` 新增註記，明示網頁類標竿需改用 `webpage-capture` 工具，後續仍沿用相同 Schema 與 MOC 索引。

### 🔵 v1.0
* **修改時間**：2026-06-22
* **說明**：
  * 初始化並安裝 `creative-director`（小D）技能。
  * 建立核心的 6 階段創意工作流與相關策略工具（如 Pollard 7-level taxonomy、HumanKind Scale 評分等）。
  * 內建 571 個經典廣告案例資料庫（`legendary-campaigns`）。
