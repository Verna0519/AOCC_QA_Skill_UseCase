# Guide_AOCCQA_testcase_conventions — Test Case 撰寫規範 / 範本

> 依 ASUS EC(MCC1)既有 test case 檔歸納的撰寫慣例,供產出一致格式的測項使用。搭配 `Definition_AOCCQA_glossary`(名詞)、`Definition_AOCCQA_traceability`(功能索引)、`QA_task_UAT-QA_classified`(歷史單)使用。

## 1. 檔案與工作表結構

一份 test case Excel 通常含下列工作表:

- **Report** — 測試摘要(功能、範圍、環境、版本、結果統計)。
- **Test case**(或 `TestCase`) — 測項主表(核心,見下方欄位)。
- **Bug list** — 測試中發現的缺陷清單。
- **Screenshot**(可含多國 `Screenshot_ES/PL/HU`) — 佐證截圖。
- 視需要:`Product list`、`時差`、各國別分頁。

## 2. Test case 主表欄位(標準）

| 欄位 | 說明 | 必填 |
|---|---|---|
| ID | 流水號 | ✓ |
| PC / Mobile / Tablet | 裝置/瀏覽器別(Chrome/Safari…),打勾或標註 | 視情況 |
| Category | 測項分類(如 import、Buy Page、Cart、Special Price、Magento…) | ✓ |
| Pre-condition | 前置條件(帳號、商品設定、資料狀態) | ✓ |
| **Test case** | 測項名稱/驗證目的(一句話講清楚要驗證什麼) | ✓ |
| **Steps** | 操作步驟(見 §4) | ✓ |
| **Expected result** | 預期結果(見 §5) | ✓ |
| Test result | Pass / Fail(多國時每國一欄:`Test result DE/ES/FR/PL…`) | ✓ |
| Note / Memo | 備註、關聯單號、已知問題 | |

> 最小可用組合為 **Test case + Steps + Expected result** 三欄;跨國測試才展開多個 Test result 欄。

## 3. 命名慣例

- **JIRA 單 / 檔名**:`[UAT-QA][國別] 功能名稱`,檔名再加 `_TestCase_YYYYMMDD`。
  - 例:`[UAT-QA][EU] Customized Bundle file upload error message adjustment_DE_testcase_20250115`
- 國別標記放在方括號:`EU / EU10 / US / CA / TW / MX / MY / PH / TR / PL / HU / CZ / RS / DE / ES / FR / NL / BE / SE / DK / FI / IT / PT`,多國以 `/` 分隔。
- 功能名稱盡量對齊 glossary 的 `feature`,方便追溯與盤點。

## 4. Steps 撰寫慣例

- 標明路徑起點,再列編號步驟。後台常見路徑:
  - 匯入:`Path: Magento > System > Data Transfer > Import`,選 `Entity Type` → 選 `Import Behavior`(Add/Delete) → 選檔 → `Check Data` → `Import`。
  - 商品設定:`Path: Magento > Catalog / Products`。
  - AOM:`Path: AOM(ECMW) > …`。
- 前台步驟:`開啟測試商品 Buy Page → 點選 … → 至 Cart Page → …`。
- 造資料/改狀態:可用 **Postman** 調整庫存(如「將庫存數調整為 0」)、或後台 Advanced Inventory 設定。
- 一步一行,動詞開頭;需要的輸入值明確標出(SKU、金額、日期格式 `YYYY/MM/DD`)。

## 5. Expected result 撰寫慣例

- 用**條件句**描述:「若 <條件> → 則 <系統行為/顯示>」。
  - 例:「若該欄位留空,應顯示 Alert message 並阻擋 Import」。
- 錯誤訊息**引用原文**(對照 `Reference_AOCCQA_quicklookup` 錯誤訊息集),例如:
  - `Same combination of [main_sku] + [sub_skus] + [website_code] + [user_group_id] already exists`
  - `CSV file larger than 2MB`
- 涉及金額/邏輯**附公式**(對照 glossary 價格公式),例如 `+$ =(main_sku_set_price + sub_skus_set_price)− 主商品最終價`。
- 涉及前後台連動時,預期結果要同時涵蓋**後台動作 → 前台呈現**(對照 `Definition_AOCCQA_relations`)。

## 6. 結果與嚴重度用語

- **Test result**:`Pass` / `Fail`(部分檔用 `Pass/NG`)。
- **JIRA 狀態**(對應驗測流程):`Open / In Progress / QA-NG / FEEDBACK / Fixed / Closed / Parking / Cancelled` 等。
- Bug 嚴重度(Bug list):依專案慣例填 `Severity`(Critical/Major/Minor…)。

## 7. 跨國 / 多語系注意

- `website_code` 與 `locale` 要對齊,且 locale 需完整(如 **BE 需 nl_BE + fr_BE**)。
- 日期/特價生效依**網站當地時區(local time)**,非台灣時間(如 PL/HU=台灣 6 點、FI=台灣 5 點)。
- 適用金流/物流依國別不同(見 `Reference_AOCCQA_quicklookup` 與 traceability)。

## 8. 撰寫前建議查詢順序(對應 CLAUDE.md 路由)

1. `glossary` — 抓該功能名詞、規則、公式、錯誤訊息原文。
2. `relations` / `system_relations` — 補前後台連動與跨系統資料流,確保預期結果涵蓋兩端。
3. `ecpages` — 確認該頁有哪些區塊要驗。
4. `traceability` + `QA_task_UAT-QA_classified` — 看歷史測過哪些、避免重複、找回歸點。
5. `EU_FSD_latest_reference` — 需要規格細節時取該章最新頁。

## 9. 空白範本(可直接複製)

| ID | Category | Pre-condition | Test case | Steps | Expected result | Test result | Note |
|---|---|---|---|---|---|---|---|
| 1 | | | | | | | |
| 2 | | | | | | | |
