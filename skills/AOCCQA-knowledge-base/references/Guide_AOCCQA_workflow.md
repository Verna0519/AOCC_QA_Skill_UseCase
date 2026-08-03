# Guide_AOCCQA_workflow — 測試需求分析→測項產出的可重複工作流程

> 把今天建立的所有知識庫串成一條可重複套用的流程,並內建 token 控制規則。所有檔在本資料夾;`kb_manifest.json` 是歸檔索引;`CLAUDE.md` 是常駐路由。此檔本身小,skill 與主代理共用。

## 0. 鐵則(token 控制)

1. **只查不載**:要定義/關係/測試點/歷史單,一律用 `bash + jq` 撈符合條件的那幾筆,**不要 Read 整個 JSON**。
   - 量級:`glossary.json` 整檔 ≈ 120K tokens;`jq` 單筆 ≈ 數十 tokens(差上千倍)。
2. **CLAUDE.md 是唯一自動載入**;其餘按需開。別把資料長進 CLAUDE.md。
3. **大量讀取/分析丟 subagent**,只回濃縮結果,原始資料留在子代理。
4. **先看路由再下手**:先用 `CLAUDE.md` 路由表或 `kb_manifest.json` 決定「開哪個檔、用哪個 key」,再下 jq。

## 1. 知識庫歸檔(今天建立的全部)

| 檔案 | 角色 | 何時用 |
|---|---|---|
| `CLAUDE.md` | 常駐索引/路由 | 自動載入 |
| `kb_manifest.json` | 歸檔索引 + 查詢食譜 | 決定開哪個檔 |
| `Definition_AOCCQA_glossary.json` | 名詞權威庫(626) | 遇到名詞/欄位/錯誤訊息/公式 |
| `Definition_AOCCQA_relations.json` | 前後台關係(238) | 後台改動影響前台哪裡 |
| `Definition_AOCCQA_system_relations.json` | AOM/Magento/EC 三層(36) | 跨系統資料流 |
| `Definition_AOCCQA_ecpages.json` | EC 頁面定義(27) | 某頁有哪些區塊 |
| `Definition_AOCCQA_traceability.json` | 功能追溯矩陣(24) | 某功能有哪些資產 |
| `Reference_AOCCQA_quicklookup.json` | 縮寫/錯誤訊息/API | 引用原文、查來源 |
| `QA_task_UAT-QA_classified.json` | QA 歷史單(776) | 測過哪些、找相似、回歸點 |
| `EU_FSD_latest_reference.json` | FSD 章節最新頁 | 對照官方規格 |
| `Guide_AOCCQA_testcase_conventions.md` | 撰寫規範/範本 | 寫測項前對齊格式 |
| `Guide_AOCCQA_testpoint_library.json` | 測試點素材(511) | 展開測項內容 |
| `Guide_AOCCQA_coverage_and_sequencing.json` | 涵蓋維度+順序準則 | 檢核漏測、排順序 |

## 2. 主流程(FSD/需求 → 測項)

> 每步只取需要的紀錄(jq),不整檔載。

1. **辨識功能**：從需求/FSD 標題判斷屬哪個功能(對照 traceability 的 24 分類)。
2. **抓定義**：遇到的名詞 → `glossary` 以 `term`/`aliases` 撈定義、公式、錯誤訊息(勿臆測)。
3. **補連動**：`relations`(後台→前台)、`system_relations`(跨系統)撈該功能相關關係,確保預期結果涵蓋兩端。
4. **確認頁面**：`ecpages` 撈涉及頁面的區塊清單。
5. **檢核涵蓋**：用 `coverage_and_sequencing` 的 12 維度逐項自問有沒有漏測。
6. **展開測試點**：`testpoint_library` 撈該功能的子區與測試點當素材（正常/邊界/異常/跨國/跨系統）。
7. **看歷史/回歸**：`QA_task_UAT-QA_classified` 撈該功能既有單,避免重複、補回歸點。
8. **要規格細節時**：`EU_FSD_latest_reference` 取該章最新頁連結，必要時用 Confluence 連接器單獨深讀該頁。
9. **排順序**：依 `coverage_and_sequencing` 的 12 階段巨觀 + 5 步微觀 + 相依原則排列測項。
10. **產出**：依 `Guide_AOCCQA_testcase_conventions` 的欄位/命名/寫法輸出 test case（存到指定資料夾）。

## 3. 低 token 查詢食譜(直接複製)

在本資料夾用 `bash` 執行(把 `X` 換成目標):

```bash
# 名詞定義
jq '.terms[]|select(.term=="Save$" or (.aliases[]?=="Save$"))' Definition_AOCCQA_glossary.json
# 某功能全部名詞(只回 term+definition)
jq '[.terms[]|select(.feature|index("Payment"))|{term,definition_zh}]' Definition_AOCCQA_glossary.json
# 後台設定影響前台哪裡
jq '.relations[]|select(.backend|test("main_sku_set_price"))' Definition_AOCCQA_relations.json
# 某功能測試點
jq '.features[]|select(.feature=="Customized Bundle")' Guide_AOCCQA_testpoint_library.json
# 某功能歷史單(只回 key+summary)
jq '[.issues[]|select(.feature=="WEP Single Buy")|{key,summary,status}]' QA_task_UAT-QA_classified.json
# 錯誤訊息原文
jq '.error_messages[]|select(.term|test("2MB"))' Reference_AOCCQA_quicklookup.json
```

> 想更省:先 `jq 'length'` / `jq 'keys'` 看結構與筆數,再取單筆。

## 4. 跟其他 skill 共用

- **共享方式**：這些檔與 `CLAUDE.md` 都在本資料夾;任何在此資料夾執行的 skill(`aoccqa-fsd-parser`、`aoccqa-tc-generator` 等)都共享同一份路由與資料,不需各自複製。
- **漸進揭露**：skill 的 SKILL.md 保持精簡,只寫「知識庫在此、依 manifest/CLAUDE.md 用 jq 按需取」,實際資料按需載,避免把大檔寫進 skill。
- **SKILL.md 可貼入的引用段**(貼到對應 skill 的 SKILL.md;skill 檔需在 Settings > Capabilities 編輯):

```md
## 知識庫(AOCCQA)
本 skill 依賴 `AOCCQA_mcc1_glossary` 資料夾的 AOCCQA 知識庫。
- 先讀 `kb_manifest.json` 決定開哪個檔、用哪個 key。
- 一律用 bash `jq` 過濾取需要的紀錄,勿 Read 整個 JSON(控 token)。
- 路由與功能索引見 `CLAUDE.md`;流程見 `Guide_AOCCQA_workflow.md`。
- 大量分析請用 subagent,只回濃縮結果。
```

- **注意**:我無法從這裡改你已安裝的 skill 檔(唯讀);上面段落請你貼到 Settings > Capabilities 的對應 skill。
