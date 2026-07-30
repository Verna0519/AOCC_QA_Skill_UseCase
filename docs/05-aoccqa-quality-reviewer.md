# aoccqa-quality-reviewer（品質審查員）

- **版本**：20.1.0（frontmatter `metadata.version`；skill 名 `aoccqa-quality-reviewer-v201`，另有舊版 `aoccqa-quality-reviewer`）
- **Phase / 產線位置**：Phase B / Gate 2（審查放行），回饋節點 ③
- **附帶檔案**：無（知識庫 12 維查詢依賴 `aoccqa-knowledge-base`）
- **原始檔備份**：[`skills/aoccqa-quality-reviewer-v201/`](../skills/aoccqa-quality-reviewer-v201/)

## 定義

擔任分析產線的**獨立第二人視角**。用 QA 交給 `aoccqa-fsd-parser` 的**同一份完整原始 INPUT**，自己重新讀一次，對 parser 產出的**六段分析報告**做雙向查核，再加上獨立推導的應有測項，整合成一份在對話視窗即時視讀的精簡報告，交回 Gate 2。

## 用途

- 逐句、逐圖、逐字雙向查核：報告每個主張是否在 INPUT 找得到依據且技術值逐字一致；INPUT 每句規格是否被反映。
- 在**讀報告之前**先獨立推導應有測項，避免被報告帶著走而一起漏測。
- 以 `aoccqa-knowledge-base`（12 涵蓋維度）輔助、分證據等級。

## 為什麼要獨立重讀

若沿用 parser 已整理好的結論來反推，本質上仍站在它的視角——它漏掉的維度會跟著漏。只有自己重新取得同一份原始 INPUT、在看報告前先推導一次，才是真正第二人視角。

## 輸入

| 項目 | 必要性 |
|---|---|
| 原始 INPUT（與 parser 收到的**完全同一份**） | 必要 |
| 來源定位（Confluence 頁 ID／Jira key／檔名） | 必要（給不出 → `BLOCKED`） |
| aoccqa-fsd-parser 六段分析報告 | 必要 |
| QA 定義的測試範圍 | 建議 |

## 輸出

A／B／C 三段報告、交付判定、差異指令（只顯示有問題項）。

## 執行順序（不可調換，順序本身是防錯機制）

1. 拆解原始 INPUT → S 單元清單
2. 獨立推導應有測項 → E 清單（此時**尚未看過報告**）
3. 讀取 aoccqa-fsd-parser 六段報告 → R 測項清單
4. 四向比對
5. 產出報告（對話視窗、只顯示有問題項）

## 使用規範（責任邊界）

- **只提問題與建議**，不改報告、不執行測試、不判 Pass/Fail、不重寫規格、不代 PM/RD/QA 決策。
- **不吃 Requirement Matrix**（刻意隔離，避免沿用 parser 視角）。
- 原始 INPUT 完全取不到 → 只能做內部一致性審查，結論一律 `BLOCKED` 並註明「本次未驗證需求正確性」。
- 不得用記憶、其他專案文件、電商慣例補缺口（知識庫推導僅列為建議）。

## 觸發詞

審查/比對這份分析報告、報告有沒有漏細節、有沒有捏造或超出文件的測項、技術值有沒有抄錯、Gate 2 審查、aoccqa-fsd-parser 報告再審一次。

## 使用情境

- parser 出的六段報告要放行前，需要有人「用原文再對一次」確認沒漏測、沒捏造、技術值沒抄錯。
- 重審上一輪已修正的報告。
