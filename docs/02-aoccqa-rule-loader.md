# aoccqa-rule-loader（規則整備）

- **版本**：frontmatter 未標版本
- **Phase / 產線位置**：Phase A｜步驟 3「規則整備」，回饋節點 ①
- **上游**：`aoccqa-fsd-parser`（Requirement Matrix）→ 步驟 2 規格確認
- **下游**：`aoccqa-tc-generator`（引用 Requirement ID / Rule ID）
- **附帶檔案**：`agents/openai.yaml`（Codex/OpenAI agent 定義）
- **原始檔備份**：[`skills/aoccqa-rule-loader/`](../skills/aoccqa-rule-loader/)

## 定義

把散落的**已確認規則**，整理成可靠、可追溯、**按市場切片**的 Rule Context。每條可用規則須答四問：規則是什麼？在哪/何時適用？哪份證據授權？是否可靠到能定義 Pass/Fail？

## 用途

當 Pass/Fail 或適用性在 Requirement Matrix 之外還取決於下列因素時使用：市場規則（國別/網站/語系/幣別/時區）、身分別（Guest/Member/Admin/系統）、產品或資料型別、狀態流轉、後台設定/資格/排除、欄位對映/列舉/空值、Job/排程/觸發、跨系統整合（前台/後台/API/SFTP/報表/Email/稽核）。

## 輸入

1. 已確認 Requirement Matrix（已過步驟 2 規格確認；未解列與被允許假設須標記）。
2. In Scope / Out of Scope 定義。
3. 市場規則庫與 Country／Product Type 條件。

> **規則按市場載入**：只取當前需求涉及的市場，不一次載入全部國別。

## 輸出（固定三件）

- **Normalized Rule Context**
- **Rule Applicability Matrix**
- **Missing／Conflict Rule Register**（待補／衝突規則）

## 使用規範（責任邊界）

- **不解析**原始 FSD/PRD/截圖/Figma/API（那是 `aoccqa-fsd-parser`）。
- **不替 PM/RD 決定**尚未確認的產品行為。
- **不從其他國別/舊專案/既有 Test Case/現行系統行為/QA 慣例推論**某市場規則。
- **不把「未提及」當成** Disabled／Unsupported／Not Applicable。
- **不產生** Coverage Gap、Test Case、Steps、Expected Result、優先級。
- **不自行選擇互相衝突的規則**；規則缺失時不得以其他市場規則頂替，回退步驟 2 由 PM/RD 補件。

## 執行閘門

- Gate 1：Requirement Matrix 未過規格確認 → 停止，回報 `Not Ready for Rule Loading`。
- Gate 2：需求已自足時回 `No Additional Rule Loading Required`。

## 觸發詞

規則載入、規則整備、整理市場規則、Rule Context、規則適用性/權威/新鮮度/衝突、載入當前市場規則。

## 使用情境

- 同一功能要上多個國別站點，Pass/Fail 會因幣別/語系/資格規則不同，需先把「這一輪涉及的市場」規則整理清楚。
- 狀態流轉 + 後台設定會決定某動作是否被允許，需建立可追溯的規則清單交給產案。
