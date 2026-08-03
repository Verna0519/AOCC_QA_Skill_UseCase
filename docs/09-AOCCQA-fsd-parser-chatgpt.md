# aoccqa-fsd-parser-chatgpt（Codex/ChatGPT 版需求 parser）

- **Phase / 產線位置**：Phase A｜需求解析（Codex/ChatGPT 環境版本）
- **語言**：英文撰寫，對應 Codex 執行環境

## 定義

`aoccqa-fsd-parser` 的 Codex/ChatGPT 對應版本。把混雜、非結構化的需求來源轉成可追溯的 QA 分析：同一輪產出已確認規則與必要釐清問題，答案回來後再維持釐清對話。以直白語言寫給 QA/PM/RD，同時保留驗證所需的技術值。最終交付為單一自包含 HTML。

## 用途

- 解析並解釋專案、抽出可追溯需求、建立 technical / QA Requirement Matrix。
- 標出 missing / ambiguous / conflicting / illogical / untestable 規格。
- 分類 QA 驗證範圍，進行 stateful 的白話釐清對話。

## 輸入（Accept Sources）

FSD、PRD、功能/API 規格、PDF、Word、Markdown、Excel、HTML、既有 test case；Figma、截圖、流程圖、Jira、Confluence、SharePoint、Google Sheets；PM/RD/QA 訊息與決策；前後版本；可存取連結。**不要求使用者填表**；只有在內容不可讀、來源衝突未解、或缺目標會實質改變分析時才提問。連結不可讀時，說明未讀取並要求貼上文字/截圖/匯出，**不從連結標題臆測需求**。

## 輸出

單一自包含 HTML 需求分析（面向使用者）。

## 使用規範（責任邊界）

- **停在需求分析與 test-planning 範圍**：**絕不**產生 test case、test-case ID、preconditions、execution steps、expected-result steps、scenario 數量或執行順序 —— 那屬 `aoccqa-tc-generator`。
- 逐份來源盤點：記錄 name / type / version-or-date / readability / role。

## 使用情境

- 在 Codex/ChatGPT 環境（而非 Claude）要做同樣的 Phase A 需求解析。
- 需要英文輸出、或與既有 Codex 工作流整合時的替代 parser。

> 與 `aoccqa-fsd-parser`（Claude 版，v1.2.0）功能對位，擇一使用即可。
