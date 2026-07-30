# AOCCQA 預期 Skill 使用流程

> 依流程圖記錄。圖例：■ Skill 自動執行｜□ QA 人工判斷｜---- 回退路徑（會反覆執行）

## 流程總表（由上而下）

| # | 執行者 | 階段 | 說明 | 產出 |
|---|--------|------|------|------|
| 1 | Skill | **AOCCQA-fsd-parser** | 解析全部來源；原始檔僅此處讀取一次 | Requirement Matrix、Source Manifest ＋ 釐清清單 |
| 2 | QA ＋ PM | **規格確認**（關卡 ②） | 唯一必經人工關卡；未取得結論不往下 | 釐清結論 |
| 3 | Skill | **AOCCQA-rule-loader**（標記 ①） | 接市場輸入；擷取當前市場規則 | Normalized Rule Context、Rule Applicability Matrix |
| 4 | Skill | **AOCCQA-tc-generator** | 引用 Requirement ID；不重複讀原始檔 | 第一版 Test Case、Coverage Matrix ＋ 追溯表 |
| 5 | Skill | **AOCCQA-scenario-expander** | 以現有素材為 Coverage Baseline | Coverage Gap Matrix、增補案例草稿 |
| 6 | Skill | **AOCCQA-quality-reviewer**（標記 ③） | 不使用 Requirement Matrix；讀原文查核 | A／B／C 三段報告、交付判定 ＋ 差異指令 |
| 7 | QA | **判定處置** | 接受／不接受（記錄理由）／需再確認 | 定稿 ＋ 案例清單 |
| 8 | Skill | **AOCCQA-case-exporter** | 任何階段皆可獨立呼叫；需附 Jira 單 | xlsx 交付檔、欄位填入狀況回報 |
| 9 | Skill ＋ QA | **AOCCQA-decision-archiver** | QA 確認後寫入；與匯出互不依賴 | .md 決策紀錄 |

## 關鍵原則

- **原始檔單次讀取**：僅 `fsd-parser`（步驟 1）與 `quality-reviewer`（步驟 6，讀原文查核）碰原文；中間產案流程一律引用 Requirement ID，不重複讀原始檔。
- **唯一人工關卡**：步驟 2「規格確認」由 QA＋PM 執行，未取得結論不得往下。
- **獨立呼叫**：`case-exporter`（步驟 8）任何階段皆可獨立呼叫；`decision-archiver`（步驟 9）與匯出互不依賴。

## 回退路徑（虛線，會反覆執行）

- ① `rule-loader`、② `規格確認`、③ `quality-reviewer` 為圖中標記的回饋節點。
- `quality-reviewer` 產出的差異指令可回退至前段（tc-generator／scenario-expander）反覆修正，直到 QA 判定處置為「接受」。
- 「判定處置」若為「需再確認」，回退至規格確認關卡。
