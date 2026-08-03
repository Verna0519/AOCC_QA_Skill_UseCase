# aoccqa-tc-generator（測試案例起草員）

- **版本**：1.5.0（版本紀錄；下游審查者更名為 `aoccqa-quality-reviewer`、上游明列 `aoccqa-rule-loader`）
- **Phase / 產線位置**：Phase B｜案例起草
- **隸屬角色**：`AOCCQA-testcase-drafter`（案例起草員）
- **上游**：`aoccqa-fsd-parser`（需求分析報告 / Requirement Matrix）＋ `aoccqa-rule-loader`（Rule Context / Rule ID）
- **下游**：`aoccqa-quality-reviewer`（審查、刪重、補漏）→ `aoccqa-case-exporter`（輸出）
- **附帶檔案**：`references/coverage-and-examples.md`（電商覆蓋維度、product type、在地化維度與完整範例）
- **原始檔備份**：[`skills/AOCCQA-tc-generator/`](../skills/AOCCQA-tc-generator/)

## 定義

把上游**已確認的需求**展開成一份**可執行、可追溯、不重複**的 Test Case Draft。把「測試設計」當主要工作 —— 不是把每條需求逐字翻成一列表格。

## 用途

規劃測試覆蓋並產生第一版測項初稿（正/負/邊界/狀態流轉覆蓋），引用 Requirement ID / Rule ID，不重讀原始檔。

## 輸入

- Requirement Matrix（含 Requirement ID 與確認狀態），或 `aoccqa-fsd-parser` 的六段需求分析報告。
- 測試目的與 In Scope／Out of Scope。
- 已確認業務規則與**明確允許**的假設。
- 範圍內的角色、站點、國家、語言、系統、product/data type、狀態；可觀察結果位置（前台/Magento/AOM/Report/DB/Log）。
- （選用）既有 Test case 分頁：**只讀 5 欄** `Category / Pre-condition / Test case / Steps / Expected result`，保留原文與順序不改寫。

## 輸出

固定 **7 欄** Test Case Draft，預設為可複製的 Markdown 表格：

`Test Case ID / Category / Feature / Pre-condition / Test Case / Steps / Expected Result`

（Test result 一律留白。）

## 使用規範（責任邊界，契約不可違反）

- 只接受**已確認需求＋明確允許的假設**；來源模糊到會改變 Expected Result 或 Pass/Fail 時，**Block 該案例並提釐清問題**，不自行裁決。
- 產出是**給 QA 審查的初稿**，不宣稱最終核准。
- **不執行測試、不填 Test result、不判 Pass/Fail。**
- **不刪除或合併既有案例**；疑似重複只**標記**交 reviewer。
- **不臆造**來源沒有的產品行為、角色、國家、product type、系統、重試、刪除、null、log。
- 不加效能/安全/負載/自動化，除非需求相關或使用者要求。
- **不重新解析**原始 FSD/PRD/截圖（屬 Phase A 的 `aoccqa-fsd-parser`）。

## 觸發詞

「把這份需求變成測試案例」「幫我產測項/測試案例初稿」「規劃測試覆蓋」「這些規則要測哪些 case」「Requirement Matrix 轉 Test Case」「補齊正/負/邊界/狀態流轉覆蓋」。

## 使用情境

- 需求與規則都確認好了，要一次把可測的測項覆蓋規劃出來（第一版）。
- 新功能沒有既有測案 baseline —— 這是產「第一版」測案的正確入口（既有測案補強請改用 `aoccqa-scenario-expander`）。
