# aoccqa-case-exporter（案例匯出）

- **版本**：frontmatter 未標版本
- **Phase / 產線位置**：Phase C｜共用工具（任何階段可獨立呼叫），與 decision-archiver 無關聯
- **性質**：確定性格式化工具（人工指令觸發、Agent 執行）
- **附帶檔案**：`assets/Test_Case_Template_Claude.xlsx`（AOCC 官方模板）、`scripts/export_test_cases.py`（匯出腳本）
- **原始檔備份**：[`skills/aoccqa-case-exporter/`](../skills/aoccqa-case-exporter/)

## 定義

把「前一個 agent（測試案例產生器）的測試案例輸出」＋「一張 Jira 單」→ 套進 AOCC QA **官方 xlsx 模板** → 匯出成 xlsx。**只搬運與套版，不判斷、不篩選、不改寫案例內容。**

## 用途

產出可交付的 Test Case xlsx 檔（含 Report 分頁自動帶入 Jira 資訊）。

## 輸入（兩份）

1. **Jira 單**（用於 Report 分頁與檔名）：Project name、Assignee、Summary、link、MCC#、Description（抓「測試時程」與「測試環境」段落）。
2. **前一個 agent 的測試案例輸出**（7 欄標準格式）：`Test Case ID, Category, Feature, Pre-condition, Test Case, Steps, Expected Result`。

## 輸出

xlsx 交付檔 + 欄位填入狀況回報。

## 使用規範（契約，不可違反）

1. **只整理輸出**：原樣填入，不增刪、不改寫、不過濾、不重排序。
2. **欄位固定**：Test case 分頁沿用模板既有 12 欄，順序與命名不變。
3. **公式不動**：Report 的 Pass/Fail/N-A 統計與比率、Bug list 嚴重度統計，保留原公式（不填值）。
4. **Bug list / Screenshot 分頁**：原樣保留。
5. 抓不到的欄位留白並告知使用者，**不臆測填值**。

### Report 分頁對應（← Jira，已鎖定）

| Report 欄位 | 來源 |
|---|---|
| Project（→C2） | Jira Summary 完整標題（含 `[UAT-QA][XX]` 標籤，原樣） |
| Test date（→C3） | Jira Description 時程，正規化為 `YYYY/MM/DD-YYYY/MM/DD` |
| Test Version（→C4） | 暫留白 |
| Tester（→C5） | `AOCCQA_{Assignee}`（待與 `AOCC_` 前綴確認） |
| New feature & Release Note（→C6） | Jira link |
| Test Country（→C13） | Jira MCC# |
| Test Environment（→C14） | Jira Description「測試環境」段落 |

> **Pending**：Tester 前綴 `AOCCQA_` vs 模板預填 `AOCC_`，待確認。

## 觸發詞

「匯出測試案例成 xlsx」「套進 AOCC 模板」「接上一個 agent 的案例輸出做成 Excel」「產出 Test Case 檔」「把案例清單存成 Test_Case 檔」，或提供 Jira 單＋測試案例要打包成交付檔。

## 使用情境

- 測案審查定稿後，要交付一份符合 AOCC 官方模板格式、Report 分頁帶好 Jira 資訊的 xlsx。
- 任何階段想先把目前案例打包成 Excel 檔存查。
