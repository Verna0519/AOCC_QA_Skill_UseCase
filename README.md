# AOCC QA Skill — 使用規範與情境（Use Case）

ASUS EC（Magento）QA 測試需求分析與 Test Case 產線所使用的一組 Claude Skill。本 repo 記錄每個 skill 的**定義、用途、使用規範（責任邊界）、輸入/輸出、觸發詞與使用情境**，並說明它們如何串接成一條產線。

---

## 產線總覽

這組 skill 對應一條「需求 → 分析 → 規則 → 產案 → 審查 → 交付 → 歸檔」的測試案例產線，分為三個 Phase：

| Phase | 階段 | 主要 Skill | 執行者 |
|-------|------|-----------|--------|
| A | 需求解析 | `aoccqa-fsd-parser` | Skill |
| A | 規格確認（★唯一必經人工關卡） | —（人工） | QA + PM |
| A | 規則整備 ① | `aoccqa-rule-loader` | Skill |
| B | 案例起草 | `aoccqa-tc-generator` | Skill |
| B | 情境擴充 | `aoccqa-scenario-expander` | Skill |
| B | 品質審查 ③（Gate 2） | `aoccqa-quality-reviewer` | Skill |
| B | 判定處置 | —（人工） | QA |
| C | 案例匯出 | `aoccqa-case-exporter` | Skill |
| D | 決策歸檔 | `aoccqa-decision-archiver`（尚未建立） | Skill + QA |

輔助：`aoccqa-knowledge-base`（跨功能知識庫，全程可查）、`aoccqa-analysis-report`（HTML 呈現層，已併入 fsd-parser）、`aoccqa-fsd-parser-chatgpt`（Codex/ChatGPT 版 parser）。

### 三大鐵則（貫穿全線）

1. **原始檔單次讀取**：只有 `fsd-parser`（解析）與 `quality-reviewer`（獨立查核原文）可接觸原始來源；中間步驟一律引用 Requirement ID，不重讀原始檔。
2. **唯一必經人工關卡**：「規格確認」由 QA＋PM 執行，未取得釐清結論不得往下。
3. **審查刻意隔離**：`quality-reviewer` 不吃 Requirement Matrix，獨立重讀原文，避免沿用 parser 視角而一起漏測。

---

## Skill 索引

| # | Skill | 版本 | Phase | 一句話定位 | 文件 |
|---|-------|------|-------|-----------|------|
| 1 | `aoccqa-fsd-parser` | 1.2.0 | A | 把任何規格來源解析成給 PM 看的六段需求分析報告 | [docs](docs/01-aoccqa-fsd-parser.md) |
| 2 | `aoccqa-rule-loader` | — | A | 把散落的市場規則整理成可追溯的 Rule Context | [docs](docs/02-aoccqa-rule-loader.md) |
| 3 | `aoccqa-tc-generator` | 1.5.0 | B | 把已確認需求展開成 7 欄 Test Case 初稿 | [docs](docs/03-aoccqa-tc-generator.md) |
| 4 | `aoccqa-scenario-expander` | — | B | 對照合格 baseline 補強覆蓋缺口 | [docs](docs/04-aoccqa-scenario-expander.md) |
| 5 | `aoccqa-quality-reviewer` | 20.1.0 | B | 第二人視角獨立重讀原文，雙向查核報告 | [docs](docs/05-aoccqa-quality-reviewer.md) |
| 6 | `aoccqa-case-exporter` | — | C | 把案例＋Jira 單套官方模板匯出 xlsx | [docs](docs/06-aoccqa-case-exporter.md) |
| 7 | `aoccqa-knowledge-base` | — | 全程 | 跨功能知識庫（名詞/關係/測試點/歷史單），**不在流程順序內、全程可查** | [docs](docs/07-aoccqa-knowledge-base.md) |

> 以上 1–7 為**現行七支核心 skill**。第 7 支 `knowledge-base` 不是流程中的一步，而是全程可查的共用知識庫。

### 附錄：變體與尚未建立

| Skill | 版本 | 說明 | 文件 |
|-------|------|------|------|
| `aoccqa-analysis-report` | 1.0.0 | 六段 HTML 報告呈現層，功能已併入 `fsd-parser` | [docs](docs/08-aoccqa-analysis-report.md) |
| `aoccqa-fsd-parser-chatgpt` | — | Codex/ChatGPT 版需求 parser（與 `fsd-parser` 對位，擇一使用） | [docs](docs/09-aoccqa-fsd-parser-chatgpt.md) |
| `aoccqa-decision-archiver` | 設計定義 | Phase D 決策歸檔（Agent 彙整＋QA 確認才寫入 `.md`）。**尚未建立為 skill**，儲存位置與分類方式尚未定案 | [docs](docs/10-aoccqa-decision-archiver.md) |

---

## 快速使用情境對照

| 我想做的事 | 用哪個 skill |
|-----------|-------------|
| 「幫我解析這份 FSD／做測試需求分析／整理要問 PM 的問題」 | `aoccqa-fsd-parser` |
| 「整理當前市場（國別/網站/語系）規則、Rule Context」 | `aoccqa-rule-loader` |
| 「把這份需求變成測試案例／產測項初稿」 | `aoccqa-tc-generator` |
| 「既有測案要補強覆蓋（狀態流轉/身分別）」 | `aoccqa-scenario-expander` |
| 「審查這份分析報告有沒有漏測/捏造/技術值抄錯」 | `aoccqa-quality-reviewer` |
| 「把案例套 AOCC 模板匯出成 xlsx」 | `aoccqa-case-exporter` |
| 「查名詞定義/後台前台關係/歷史單」 | `aoccqa-knowledge-base` |

---

## 使用情境（不是每次都七支全用）

同一條產線，依情境決定哪幾步會用、跳過或重複。互動視覺版見 [`diagrams/AOCCQA_skill_roles_and_scenarios.html`](diagrams/AOCCQA_skill_roles_and_scenarios.html)。

| 情境 | 說明 | 使用的步驟 |
|------|------|-----------|
| 1. 新需求·跨市場·完整流程 | 全新功能、多市場、有相鄰既有案例可補強 | 七支全用 |
| 2. 新需求·單一市場·無既有案例（最常見） | 單一市場、無舊案例 → 跳過 rule-loader、scenario-expander | fsd-parser → 規格確認 → tc-generator → quality-reviewer → 判定 → case-exporter |
| 3. 既有功能異動·補強路徑 | 已有完整舊案例 → 以補強為主，不重建第一版 | 跳過 tc-generator，改走 scenario-expander |
| 4. 規格不足·早停 | fsd-parser 回 BLOCKED → 後面全停，先補件 | 只用 fsd-parser |
| 5. 審查判定 REWORK·回流 | reviewer 找到漏測 → tc-generator 只補差異、reviewer 只查新增 | tc-generator / quality-reviewer 重複呼叫 |
| 6. 單點呼叫 | skill 可獨立呼叫：case-exporter 只出檔 / quality-reviewer 健檢 / fsd-parser 整理釐清問題 | 只用其中一支 |

**三條最重要、也最容易被破壞的設計**：規格沒確認不往下（情境 4）、reviewer 不看需求清單才審得出問題、出檔只搬不改。

---

## 原始檔備份（skills/）

[`skills/`](skills/) 收錄 7 支 skill 的原始 `SKILL.md` 與其 references／scripts／assets，作為版本備份：

| Skill | 版本 | 附帶檔案 |
|-------|------|---------|
| `aoccqa-fsd-parser` | 1.2.0 | `references/report-template.html` |
| `aoccqa-rule-loader` | 未標版本 | `agents/openai.yaml` |
| `aoccqa-tc-generator` | 1.5.0 | `references/coverage-and-examples.md` |
| `aoccqa-scenario-expander` | 未標版本 | — |
| `aoccqa-quality-reviewer-v201` | 20.1.0 | — |
| `aoccqa-case-exporter` | 未標版本 | `assets/Test_Case_Template_Claude.xlsx`、`scripts/export_test_cases.py` |
| `aoccqa-knowledge-base` | 未標版本 | `references/` 13 個資料檔（glossary 約 472K 等） |

> `fsd-parser` 為雲端 skill，備份內容依當前 v1.2.0 逐字重建；其餘 6 支由本機 skill 目錄原樣複製。

---

## 相關文件

- **流程圖（視覺版）**：[`diagrams/AOCCQA_flow_diagram.html`](diagrams/AOCCQA_flow_diagram.html) — 泳道 + 回流路徑 SVG
- **角色與使用情境（視覺版）**：[`diagrams/AOCCQA_skill_roles_and_scenarios.html`](diagrams/AOCCQA_skill_roles_and_scenarios.html)
- [完整產線流程圖說明（文字版）](workflow.md)
- [專案自訂指示（orchestration）](custom-instructions.md)
- 各 skill 詳細說明見 [`docs/`](docs/)
