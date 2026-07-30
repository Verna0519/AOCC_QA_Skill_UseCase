---
name: aoccqa-knowledge-base
description: ASUS EC (Magento) QA 測試需求分析與 test case 撰寫用的跨功能知識庫——名詞定義、前後台與 AOM/Magento/EC 三層關係、EC 頁面、功能追溯、QA 歷史單、撰寫規範、測試點素材與涵蓋/順序準則。解析 FSD/需求或撰寫測項時使用。
---

# AOCCQA Knowledge Base

ASUS EC(Magento)測試需求分析與 test case 撰寫的跨功能知識庫。所有資料在 `references/`,已是權威原檔。

## Token 鐵則(務必遵守)

1. **只查不載**:要定義/關係/測試點/歷史單,用 bash `jq`/`grep` 從 `references/` 撈**符合條件的那幾筆**,**勿 Read 整個大 JSON**(glossary 整檔≈120K tokens;jq 單筆≈數十)。
2. 先讀 `references/kb_manifest.json`(小,3KB)決定「開哪個檔、用哪個 key」,再下 jq。
3. 大量讀取/分析交給 subagent,只回濃縮結果。
4. 找不到就明講「庫內沒有」,不臆測;不要把資料整檔貼回對話。

## 何時開哪個檔(路由)

| 需求 | 檔案(references/) | 查法 |
|---|---|---|
| 名詞/欄位/錯誤訊息/公式定義 | `Definition_AOCCQA_glossary.json` | 比對 `term`/`aliases` |
| 後台設定影響前台哪裡 / 反查 | `Definition_AOCCQA_relations.json` | 比對 `backend`/`frontend` |
| 跨系統資料流(Magento↔AOM↔EC) | `Definition_AOCCQA_system_relations.json` | `from_layer`/`to_layer` |
| 某頁有哪些區塊 | `Definition_AOCCQA_ecpages.json` | `page` |
| 某功能有哪些資產/覆蓋 | `Definition_AOCCQA_traceability.json` | `feature` |
| 縮寫/錯誤訊息原文/API 來源 | `Reference_AOCCQA_quicklookup.json` | `acronyms`/`error_messages`/`apis` |
| 某功能測過哪些/歷史單/回歸 | `QA_task_UAT-QA_classified.json` | `feature`/`country`/`status` |
| 對照官方 FSD 規格 | `EU_FSD_latest_reference.json` | `chapter` |
| 展開測項內容(測試點素材) | `Guide_AOCCQA_testpoint_library.json` | `feature` |
| 檢核漏測/排測項順序 | `Guide_AOCCQA_coverage_and_sequencing.json` | 整份小,可讀 |
| 對齊測項格式/欄位/命名 | `Guide_AOCCQA_testcase_conventions.md` | 寫測項前讀一次 |
| 完整工作流程 SOP | `Guide_AOCCQA_workflow.md` | 需要時讀 |

## 寫測項流程(簡版)

辨識功能 → glossary 抓定義/公式 → relations/system_relations 補連動 → ecpages 確認頁面 → coverage 檢核漏測 → testpoint_library 展開素材 → QA_task 看歷史/回歸 → 依 sequencing 排順序 → 依 conventions 產出。詳見 `references/Guide_AOCCQA_workflow.md`。

## 低 token jq 食譜

```bash
# 名詞定義(單筆)
jq '.terms[]|select(.term=="Save$" or (.aliases[]?=="Save$"))' references/Definition_AOCCQA_glossary.json
# 某功能名詞(只回 term+定義)
jq '[.terms[]|select(.feature|index("Payment"))|{term,definition_zh}]' references/Definition_AOCCQA_glossary.json
# 某功能測試點
jq '.features[]|select(.feature=="Customized Bundle")' references/Guide_AOCCQA_testpoint_library.json
# 某功能歷史單(只回 key+summary)
jq '[.issues[]|select(.feature=="WEP Single Buy")|{key,summary,status}]' references/QA_task_UAT-QA_classified.json
# 錯誤訊息原文
jq '.error_messages[]|select(.term|test("2MB"))' references/Reference_AOCCQA_quicklookup.json
```

先 `jq 'keys'` 或 `jq '.terms|length'` 看結構/筆數,再取單筆,最省。
