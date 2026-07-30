# aoccqa-knowledge-base（知識庫）

- **版本**：frontmatter 未標版本
- **Phase / 產線位置**：全程可查（輔助 skill）
- **資料**：權威原檔放 `references/`（13 個檔）
- **附帶檔案**：`references/` — `kb_manifest.json`、`Definition_AOCCQA_glossary.json`（約 472K）、`Definition_AOCCQA_relations.json`、`Definition_AOCCQA_system_relations.json`、`Definition_AOCCQA_ecpages.json`、`Definition_AOCCQA_traceability.json`、`Reference_AOCCQA_quicklookup.json`、`QA_task_UAT-QA_classified.json`、`EU_FSD_latest_reference.json`、`Guide_AOCCQA_testpoint_library.json`、`Guide_AOCCQA_coverage_and_sequencing.json`、`Guide_AOCCQA_testcase_conventions.md`、`Guide_AOCCQA_workflow.md`
- **原始檔備份**：[`skills/aoccqa-knowledge-base/`](../skills/aoccqa-knowledge-base/)

## 定義

ASUS EC（Magento）測試需求分析與 test case 撰寫的**跨功能知識庫**：名詞定義、前後台與 AOM/Magento/EC 三層關係、EC 頁面、功能追溯、QA 歷史單、撰寫規範、測試點素材與涵蓋/順序準則。

## 用途

解析 FSD/需求或撰寫測項時，查名詞定義、後台↔前台連動、跨系統資料流、頁面區塊、功能覆蓋、歷史單/回歸、撰寫格式與漏測檢核。

## 使用規範（Token 鐵則）

1. **只查不載**：用 bash `jq`/`grep` 從 `references/` 撈符合條件的**那幾筆**，勿 Read 整個大 JSON（glossary 整檔≈120K tokens；jq 單筆≈數十）。
2. 先讀 `references/kb_manifest.json`（≈3KB）決定「開哪個檔、用哪個 key」，再下 jq。
3. 大量讀取交給 subagent，只回濃縮結果。
4. 找不到就明講「庫內沒有」，不臆測，不整檔貼回對話。

## 路由（何時開哪個檔）

| 需求 | 檔案（references/） |
|---|---|
| 名詞/欄位/錯誤訊息/公式定義 | `Definition_AOCCQA_glossary.json` |
| 後台設定影響前台哪裡 / 反查 | `Definition_AOCCQA_relations.json` |
| 跨系統資料流（Magento↔AOM↔EC） | `Definition_AOCCQA_system_relations.json` |
| 某頁有哪些區塊 | `Definition_AOCCQA_ecpages.json` |
| 某功能有哪些資產/覆蓋 | `Definition_AOCCQA_traceability.json` |
| 縮寫/錯誤訊息原文/API 來源 | `Reference_AOCCQA_quicklookup.json` |
| 某功能測過哪些/歷史單/回歸 | `QA_task_UAT-QA_classified.json` |
| 對照官方 FSD 規格 | `EU_FSD_latest_reference.json` |
| 展開測項內容（測試點素材） | `Guide_AOCCQA_testpoint_library.json` |
| 檢核漏測/排測項順序 | `Guide_AOCCQA_coverage_and_sequencing.json` |
| 對齊測項格式/欄位/命名 | `Guide_AOCCQA_testcase_conventions.md` |
| 完整工作流程 SOP | `Guide_AOCCQA_workflow.md` |

## 使用情境

- 撰寫測項時要抓某個名詞/公式的權威定義，或確認某後台設定會連動前台哪些區塊。
- 檢核某功能歷史上測過哪些、是否有回歸單可沿用。
- 被 `quality-reviewer` 用於獨立推導 12 涵蓋維度。
