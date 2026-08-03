# 環境需求（SETUP）

依「你想做什麼」決定要裝什麼。**只是看文件／流程圖 → 什麼都不用裝。**

| 使用層級 | 需要的環境 | 安裝方式 |
|---|---|---|
| **看** 文件與流程圖（`README.md`、`docs/`、`diagrams/*.html`） | 瀏覽器即可（HTML 為自包含單檔）；或用 GitHub Pages 線上看 | 免安裝 |
| **執行** skill 產線（各 `SKILL.md`） | **Claude Code** 或 claude.ai（支援 skills 的環境） | 你既有的 Claude 環境 |
| **查詢** 知識庫（`skills/AOCCQA-knowledge-base/references/*.json`） | `jq` + `bash`／`grep` | 見下方 |
| **出檔** 測試案例 xlsx（`AOCCQA-case-exporter` 的 `scripts/export_test_cases.py`） | **Python 3** + `openpyxl` | 見下方 |
| （選用）OpenAI Codex/Agent（`agents/openai.yaml`） | OpenAI 的 agent 執行環境 | 視需求 |

## 安裝指令

**jq（知識庫 JSON 查詢用）**
```bash
# Windows（winget）
winget install jqlang.jq
# macOS
brew install jq
# Debian/Ubuntu
sudo apt-get install jq
```

**Python 套件（出 xlsx 用）**
```bash
pip install openpyxl
```
> `export_test_cases.py` 其餘 import（`argparse`、`copy`、`json`、`os`、`re`、`sys`、`datetime`）皆為 Python 標準庫，無需另裝。

## 平台備註

- Windows 建議用 **Git Bash** 跑 `jq`／`grep` 食譜（`SKILL.md` 內的指令為 bash 語法）。
- 知識庫鐵則「只查不載」：用 `jq`/`grep` 撈符合條件的那幾筆，勿整檔讀取大型 JSON（`glossary` 整檔很大）。
- 各 `SKILL.md` 的 `name:`（skill 實際呼叫 ID）為小寫 `aoccqa-*`；資料夾／檔名／文件顯示名稱則統一為大寫 `AOCCQA-*`。

## 快速檢查目前環境
```bash
python --version          # 需 Python 3
python -c "import openpyxl; print(openpyxl.__version__)"
jq --version
git --version
```
