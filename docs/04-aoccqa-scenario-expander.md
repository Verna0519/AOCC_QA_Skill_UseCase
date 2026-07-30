# aoccqa-scenario-expander（情境擴充器）

- **版本**：frontmatter 未標版本
- **Phase / 產線位置**：Phase B｜補強環節（aoccqa-tc-generator 之後、aoccqa-quality-reviewer 之前）
- **產線**：`aoccqa-fsd-parser` → `aoccqa-tc-generator` → **`aoccqa-scenario-expander`** → `aoccqa-quality-reviewer`
- **附帶檔案**：無（知識庫查詢依賴 `aoccqa-knowledge-base`）
- **原始檔備份**：[`skills/aoccqa-scenario-expander/`](../skills/aoccqa-scenario-expander/)

## 定義

強化一份**合格的既有 Test Case Baseline**，找出可追溯、有證據的覆蓋缺口。**前景化「狀態流轉」與「身分別」兩軸**，再掃其餘適用維度。擴充的是**測試邏輯**，不是產品需求。

## 用途

當既有或改版功能已有審過/維護中/執行過/前版本的測案、需要更強覆蓋時使用；或用來補強一份已獨立審查過的 `aoccqa-tc-generator` 草稿的缺口。

## 輸入

| 項目 | 必要性 |
|---|---|
| 已確認 / 帶狀態標記的 Requirement Matrix | 必要 |
| 合格的 Existing Test Case Baseline | 必要 |
| In Scope / Out of Scope | 必要 |
| Normalized Rule Context + Rule ID | 適用時必要 |
| Test Data / 環境 / 抽樣 / 受保護案例 | 選填 |

## 輸出

`Supplementary Test Case Draft`（補充案例草稿、既有案例強化建議、參數化 Test Data），一律交回 `aoccqa-quality-reviewer` 審查。

## 使用規範（責任邊界）

- 以**已確認 Requirement Matrix** 為功能行為唯一權威；既有 Test Case **只當覆蓋基線**，不得當作未載明產品行為的來源。
- 只有在條件、觸發、觀察點、Expected Result 都有依據時才新增/強化情境。
- 未定義或衝突的結果一律標 `Blocked`；不得把 QA 經驗或歷史行為當產品事實。
- **不產第一版完整測項** → 路由 `aoccqa-tc-generator`。
- **不解析原始** FSD/PRD/截圖/Figma/API → 路由 `aoccqa-fsd-parser`。
- **不靜默改寫、刪除、合併、重排、重新編號或放行**既有 Test Case。

## Baseline 資格判定（關鍵）

**接受**：維護中的回歸測項、前一版測項、經 QA 審查/執行過的測項、上線/正式核准測項、QA 指定的 baseline。

**拒絕/暫停**：同一輪 aoccqa-tc-generator 剛產且未獨立審查的草稿、僅供自我比對的草稿、條件不足以判斷覆蓋的清單、與當前範圍無關的測項。

> 無合格 baseline → 回 `Needs Baseline`，改走 `aoccqa-tc-generator`（本 skill 不建立第一版測案）。

## 觸發詞

補強覆蓋、既有測案補缺口、狀態流轉/身分別覆蓋、對照 baseline 擴充情境。

## 使用情境

- 一個既有功能改版，手上有上一版審過的測案，要針對新規則補「狀態流轉 × 身分別」的缺口。
- 產線剛出的 aoccqa-tc-generator 草稿已獨立審查過，想再補強覆蓋深度。
