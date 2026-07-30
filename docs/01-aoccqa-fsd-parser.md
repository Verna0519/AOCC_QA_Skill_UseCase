# aoccqa-fsd-parser（需求解析）

- **版本**：1.2.0（frontmatter `metadata.version`）
- **Phase / 產線位置**：Phase A｜步驟 1（需求輸入與分析），全線起點
- **隸屬角色**：`AOCCQA-testcase-requirements-analyst`（需求拆解員）
- **附帶檔案**：`references/report-template.html`（六段報告 HTML 骨架）
- **原始檔備份**：[`skills/aoccqa-fsd-parser/`](../skills/aoccqa-fsd-parser/)

## 定義

把 QA 提供的**任何形式**規格資料，解析成一份**給 PM/Planner 閱讀**的測試需求分析報告（單一自包含 HTML）。此 skill 一體包含「解析分析」與「HTML 報告產出」，已併入原 `aoccqa-analysis-report` 功能，無需其他 skill 搭配。

## 用途

- 忠實反映規格**最終版本**（處理刪除線、紅字/修訂標記、inline 註解）。
- 清楚區分「已定義／需釐清／邏輯矛盾」三態。
- 偵測 spec gap（規格缺口）、來源衝突、圖文不一致。
- 整理出可直接複製發送給 PM/RD 的釐清問題。

## 輸入

Confluence FSD/PRD 連結、Jira ticket、Excel、會議記錄、截圖、Figma、API 規格，或直接貼上的文字。優先用 Confluence 連結（取 storage 原始碼）而非匯出的 PDF/純文字，以保留刪除線與紅字語意。

## 輸出

單一自包含 HTML，固定六段（順序固定）：

1. 測試目的
2. 測試目標
3. 測試範圍
4. 測試規劃
5. 待釐清
6. 邏輯不對之處（永遠放最後）

另產出 Source Manifest（逐份來源可追溯）與釐清問題清單。

## 使用規範（責任邊界）

- **不自行補寫規格**：明確區分已定義／合理推論（標「推論」）／需 PM/RD 確認，絕不把推論當已確認。
- **矛盾只列不裁決**：前後打架時列雙方原文與來源，交 PM 決定。
- **不因畫面出現某欄位就推定它是新增或有顯示條件**：沒寫的就是缺口。
- **不產 Test Case 明細**（屬 Phase B 的 `tc-generator`）。
- 所有專有名詞一律依「當前這份文件」動態抓取，不套用其他文件或範例詞彙。

## 觸發詞

「幫我解析這份 FSD」「做測試需求分析」「這份規格有沒有缺漏/邏輯不對」「整理要問 PM 的問題」「輸出成 HTML 報告」（即使未說出 "fsd-parser"）。

## 使用情境

- PM 丟來一份 Confluence FSD，QA 要先弄清楚「這份規格到底要測什麼、哪裡沒講清楚」。
- 規格改版後想確認最終版本、找出被刪除/被修訂取代的內容。
- 截圖與內文對不上，需要一份可追溯的圖文不一致清單去問 PM。

## 上下游

- **下游**：步驟 2 規格確認（QA＋PM）→ `aoccqa-rule-loader` / `aoccqa-tc-generator`。
- 被 `aoccqa-quality-reviewer` 以第二人視角重新查核。
