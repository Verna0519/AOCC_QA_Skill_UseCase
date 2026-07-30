# 推送到 GitHub 指南

目標 repo：https://github.com/Verna0519/AOCC_QA_Skill_UesCase

> 注意：這個資料夾在 OneDrive 同步位置，OneDrive 會鎖住 `.git` 內部檔案，導致我（在雲端環境）無法在此直接完成 git 操作。請在你自己的電腦上執行以下其中一種方式。

---

## 方式 A：直接在此資料夾推送（最簡單）

在此資料夾開啟終端機（PowerShell 或 Git Bash）：

```powershell
cd "C:\Users\verna_chen\OneDrive - ASUS\Claude\Run_Skill_TestResult\TEST_1\AOCC_QA_Skill_UesCase"

# 先移除我這邊殘留、未完成的 .git（若存在）
rmdir /s /q .git   # PowerShell 用：Remove-Item -Recurse -Force .git

git init
git add -A
git commit -m "docs: AOCCQA QA skill 使用規範、定義與使用情境"
git branch -M main
git remote add origin https://github.com/Verna0519/AOCC_QA_Skill_UesCase.git
git push -u origin main
```

若 repo 已有內容而被擋，改用 `git push -u origin main --force`（會覆蓋遠端），或先 `git pull --rebase origin main` 再 push。

---

## 方式 B：用我打包好的 bundle（完整含 commit 紀錄）

我已產生 `AOCC_QA_Skill_UesCase.bundle`（在輸出檔案中）。把它放到任意工作目錄後：

```bash
git clone AOCC_QA_Skill_UesCase.bundle AOCC_QA_Skill_UesCase
cd AOCC_QA_Skill_UesCase
git remote set-url origin https://github.com/Verna0519/AOCC_QA_Skill_UesCase.git
# 若尚無 origin：git remote add origin https://github.com/Verna0519/AOCC_QA_Skill_UesCase.git
git push -u origin main
```

---

## 方式 C：用 GitHub CLI（若已安裝 gh）

```bash
cd "C:\Users\verna_chen\OneDrive - ASUS\Claude\Run_Skill_TestResult\TEST_1\AOCC_QA_Skill_UesCase"
rmdir /s /q .git
git init && git add -A && git commit -m "docs: AOCCQA QA skill"
gh repo create Verna0519/AOCC_QA_Skill_UesCase --public --source=. --remote=origin --push
```

---

## 讓我直接幫你推送？

若你在 claude.ai 連結器設定裡授權 **GitHub connector**，之後在互動 session 我就能直接幫你建立/推送，不需你手動下指令。目前此 session 無法進行授權流程。
