# Git 與 GitHub CLI 完整教學指南

本指南涵蓋 Git 版本控制和 GitHub CLI 的完整使用方法，從安裝到進階應用。

## 目錄

- [Git 基礎概念](#git-基礎概念)
- [Git 安裝與設定](#git-安裝與設定)
- [Git 基本操作](#git-基本操作)
- [第一次使用 Git](#第一次使用-git)
- [日常 Git 工作流程](#日常-git-工作流程)
- [GitHub CLI 安裝與使用](#github-cli-安裝與使用)
- [分支管理](#分支管理)
- [遠端倉庫操作](#遠端倉庫操作)
- [進階 Git 技巧](#進階-git-技巧)
- [常見問題與解決方案](#常見問題與解決方案)
- [最佳實踐](#最佳實踐)

---

## Git 基礎概念

### 什麼是 Git？

Git 是一個**分散式版本控制系統**，用於追蹤程式碼的變更歷史。

**核心概念：**
- **Repository（倉庫）**：專案的所有檔案和歷史記錄
- **Commit（提交）**：一個程式碼快照
- **Branch（分支）**：獨立的開發線
- **Remote（遠端）**：雲端的 Git 倉庫（如 GitHub）

### Git 的三個區域

```
工作目錄          暫存區           本地倉庫
(Working Dir)    (Staging Area)   (Repository)
    |                |                |
    |   git add      |   git commit   |   git push
    |--------------->|--------------->|------------> 遠端倉庫
    |                |                |
    | git restore    | git restore    |   git pull
    |<---------------|   --staged     |<------------
```

1. **工作目錄**：你正在編輯的檔案
2. **暫存區**：準備提交的檔案
3. **本地倉庫**：已提交的歷史記錄
4. **遠端倉庫**：GitHub 等雲端服務

---

## Git 安裝與設定

### 安裝 Git

#### Windows
```powershell
# 方法 1: 使用 winget（推薦）
winget install --id Git.Git

# 方法 2: 下載安裝檔
# 前往 https://git-scm.com/download/win
```

#### macOS
```bash
# 方法 1: 使用 Homebrew
brew install git

# 方法 2: Xcode Command Line Tools（macOS 內建）
xcode-select --install
```

#### Linux
```bash
# Debian/Ubuntu
sudo apt-get install git

# Fedora
sudo dnf install git

# Arch Linux
sudo pacman -S git
```

### 驗證安裝

```bash
git --version
# 應該顯示類似：git version 2.42.0
```

---

### Git 初始設定

#### 1. 設定使用者資訊（必要）

```bash
# 全域設定（所有專案）
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 單一專案設定（只對當前專案有效）
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

#### 2. 設定預設編輯器

```bash
# VSCode
git config --global core.editor "code --wait"

# Vim
git config --global core.editor "vim"

# Nano
git config --global core.editor "nano"
```

#### 3. 設定預設分支名稱

```bash
# 將預設分支從 master 改為 main
git config --global init.defaultBranch main
```

#### 4. 設定換行符號處理（Windows 使用者）

```bash
# Windows 自動轉換換行符號
git config --global core.autocrlf true

# macOS/Linux 不轉換
git config --global core.autocrlf input
```

#### 5. 設定別名（shortcuts）

```bash
# 常用別名
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg 'log --oneline --graph --decorate --all'
```

#### 查看所有設定

```bash
# 查看所有設定
git config --list

# 查看特定設定
git config user.name
git config user.email

# 查看設定檔位置
git config --list --show-origin
```

---

## Git 基本操作

### 基本命令速查表

| 命令 | 說明 |
|------|------|
| `git init` | 初始化 Git 倉庫 |
| `git clone <url>` | 複製遠端倉庫 |
| `git status` | 查看狀態 |
| `git add <file>` | 加入暫存區 |
| `git commit -m "msg"` | 建立提交 |
| `git log` | 查看歷史 |
| `git diff` | 查看變更 |
| `git push` | 推送到遠端 |
| `git pull` | 從遠端拉取 |
| `git branch` | 分支操作 |
| `git checkout` | 切換分支/恢復檔案 |
| `git merge` | 合併分支 |

---

## 第一次使用 Git

### 情境 1：在現有專案中初始化 Git

```bash
# 1. 進入專案目錄
cd my-project

# 2. 初始化 Git 倉庫
git init
# 輸出：Initialized empty Git repository in /path/to/my-project/.git/

# 3. 查看狀態
git status
# 顯示所有未追蹤的檔案

# 4. 建立 .gitignore（重要！）
echo "__pycache__/" > .gitignore
echo "*.pyc" >> .gitignore
echo ".venv/" >> .gitignore

# 5. 加入所有檔案到暫存區
git add .

# 6. 建立第一個 commit
git commit -m "Initial commit"

# 7. 查看歷史
git log
```

### 情境 2：從 GitHub 複製專案

```bash
# 1. 複製倉庫
git clone https://github.com/username/repository.git

# 2. 進入目錄
cd repository

# 3. 查看遠端設定
git remote -v

# 4. 開始工作
# ...編輯檔案...

# 5. 提交變更
git add .
git commit -m "Add new feature"
git push
```

---

## 日常 Git 工作流程

### 標準工作流程

```bash
# 1. 確保在正確的分支
git branch
# * main  （* 表示當前分支）

# 2. 拉取最新程式碼（團隊協作時）
git pull

# 3. 開始工作，修改檔案
# ...編輯程式碼...

# 4. 查看變更
git status
# 顯示：modified, untracked 檔案

# 5. 查看具體變更內容
git diff
# 顯示詳細的程式碼差異

# 6. 加入特定檔案到暫存區
git add file1.py file2.py
# 或加入所有變更
git add .

# 7. 查看暫存區狀態
git status
# 顯示將要提交的檔案

# 8. 建立 commit
git commit -m "Add login functionality"

# 9. 推送到遠端
git push

# 10. 確認推送成功
git status
# 顯示：Your branch is up to date with 'origin/main'
```

### 查看變更

```bash
# 查看工作目錄的變更（尚未加入暫存區）
git diff

# 查看暫存區的變更（已 add 但未 commit）
git diff --staged
# 或
git diff --cached

# 查看特定檔案的變更
git diff file.py

# 查看兩個 commit 之間的差異
git diff commit1 commit2

# 查看統計資訊
git diff --stat
```

### 查看歷史

```bash
# 查看詳細歷史
git log

# 簡潔格式（一行顯示）
git log --oneline

# 圖形化顯示（顯示分支）
git log --oneline --graph --all

# 查看最近 5 筆
git log -5

# 查看特定檔案的歷史
git log -- file.py

# 查看誰改了什麼（blame）
git blame file.py

# 搜尋 commit 訊息
git log --grep="bug fix"

# 查看特定作者的 commit
git log --author="John"
```

---

## GitHub CLI 安裝與使用

### 什麼是 GitHub CLI？

GitHub CLI (`gh`) 是 GitHub 官方的命令列工具，讓你直接在終端機操作 GitHub。

### 安裝 GitHub CLI

#### Windows
```powershell
# 使用 winget
winget install --id GitHub.cli

# 使用 Scoop
scoop install gh

# 使用 Chocolatey
choco install gh
```

#### macOS
```bash
# 使用 Homebrew
brew install gh
```

#### Linux
```bash
# Debian/Ubuntu
sudo apt install gh

# Fedora
sudo dnf install gh

# Arch Linux
sudo pacman -S github-cli
```

### 驗證安裝

```bash
gh --version
# gh version 2.40.0 (2024-01-10)
```

---

### GitHub CLI 初始設定

#### 1. 登入 GitHub

```bash
gh auth login
```

**互動式登入流程：**
```
? What account do you want to log into?
> GitHub.com

? What is your preferred protocol for Git operations?
> HTTPS

? Authenticate Git with your GitHub credentials?
> Yes

? How would you like to authenticate GitHub CLI?
> Login with a web browser

! First copy your one-time code: XXXX-XXXX
Open this URL to continue in your web browser: https://github.com/login/device
```

**步驟：**
1. 複製一次性代碼
2. 打開瀏覽器訪問 https://github.com/login/device
3. 貼上代碼
4. 授權 GitHub CLI
5. 完成！

#### 2. 驗證登入狀態

```bash
gh auth status
```

輸出：
```
github.com
  ✓ Logged in to github.com as username
  ✓ Git operations for github.com configured to use https protocol.
  ✓ Token: *******************
```

---

### GitHub CLI 基本操作

#### Repository 操作

```bash
# 在 GitHub 建立新 repository
gh repo create my-project --public --source=. --remote=origin

# 建立私人 repository
gh repo create my-project --private --source=. --remote=origin

# 建立 repository 並推送
gh repo create my-project --public --source=. --push

# 查看 repository 資訊
gh repo view

# 在瀏覽器開啟 repository
gh repo view --web

# 複製 repository
gh repo clone username/repository

# 列出所有 repositories
gh repo list

# Fork repository
gh repo fork owner/repository

# 刪除 repository（危險！）
gh repo delete owner/repository
```

#### Issue 操作

```bash
# 列出所有 issues
gh issue list

# 查看特定 issue
gh issue view 123

# 建立 issue
gh issue create --title "Bug: Login fails" --body "Description here"

# 在瀏覽器建立 issue
gh issue create --web

# 關閉 issue
gh issue close 123

# 重新開啟 issue
gh issue reopen 123
```

#### Pull Request 操作

```bash
# 建立 pull request
gh pr create --title "Add new feature" --body "Description"

# 在瀏覽器建立 PR
gh pr create --web

# 列出所有 PR
gh pr list

# 查看 PR
gh pr view 456

# 在瀏覽器查看 PR
gh pr view 456 --web

# 檢出 PR 到本地
gh pr checkout 456

# 合併 PR
gh pr merge 456

# 查看 PR 的變更
gh pr diff 456

# 查看 PR 檢查狀態
gh pr checks 456
```

#### 其他常用命令

```bash
# 查看 release
gh release list
gh release view v1.0.0

# 建立 release
gh release create v1.0.0

# 查看 workflow runs
gh run list
gh run view 12345

# 查看 gist
gh gist list
gh gist create file.py

# 搜尋 repositories
gh search repos "python machine learning"
```

---

## 分支管理

### 分支基本概念

分支讓你可以在不影響主程式碼的情況下開發新功能。

```
main ----A----B----C----D
              \
               E----F  (feature-branch)
```

### 分支操作

```bash
# 查看所有分支
git branch
# * main
#   feature-login

# 查看遠端分支
git branch -r

# 查看所有分支（包含遠端）
git branch -a

# 建立新分支
git branch feature-new

# 建立並切換到新分支
git checkout -b feature-new
# 或使用新語法
git switch -c feature-new

# 切換分支
git checkout feature-new
# 或
git switch feature-new

# 重新命名分支
git branch -m old-name new-name

# 刪除分支（已合併）
git branch -d feature-old

# 強制刪除分支（未合併）
git branch -D feature-old

# 刪除遠端分支
git push origin --delete feature-old
```

### 合併分支

```bash
# 切換到目標分支（通常是 main）
git checkout main

# 確保是最新版本
git pull

# 合併 feature 分支
git merge feature-login

# 如果有衝突，解決後：
git add .
git commit -m "Merge feature-login"

# 推送合併結果
git push

# 刪除已合併的分支
git branch -d feature-login
```

### 分支工作流程範例

```bash
# 1. 從 main 建立新分支
git checkout main
git pull
git checkout -b feature/user-profile

# 2. 開發功能
# ...編輯檔案...
git add .
git commit -m "Add user profile page"

# 3. 推送到遠端
git push -u origin feature/user-profile

# 4. 在 GitHub 建立 Pull Request
gh pr create --title "Add user profile" --web

# 5. Code review 後，合併到 main
# （在 GitHub 網頁上點擊 Merge）

# 6. 更新本地 main 並刪除分支
git checkout main
git pull
git branch -d feature/user-profile
```

---

## 遠端倉庫操作

### 連結遠端倉庫

```bash
# 查看遠端設定
git remote -v

# 新增遠端倉庫
git remote add origin https://github.com/username/repo.git

# 修改遠端 URL
git remote set-url origin https://github.com/username/new-repo.git

# 移除遠端
git remote remove origin

# 重新命名遠端
git remote rename origin upstream
```

### 推送與拉取

```bash
# 推送到遠端（第一次需要 -u）
git push -u origin main

# 之後只需要
git push

# 推送所有分支
git push --all

# 推送標籤
git push --tags

# 從遠端拉取（fetch + merge）
git pull

# 只下載不合併
git fetch

# 查看遠端分支
git branch -r

# 追蹤遠端分支
git checkout -b local-branch origin/remote-branch
```

### 第一次推送到 GitHub

```bash
# 方法 1: 使用 GitHub CLI（最簡單）
gh repo create my-project --public --source=. --push

# 方法 2: 手動設定
# 1. 在 GitHub 建立 repository（不要初始化）
# 2. 連結遠端
git remote add origin https://github.com/username/my-project.git
# 3. 推送
git branch -M main  # 確保分支名稱是 main
git push -u origin main
```

---

## 進階 Git 技巧

### 修改 Commit

```bash
# 修改最後一次 commit 訊息
git commit --amend -m "New commit message"

# 加入遺漏的檔案到最後一次 commit
git add forgotten-file.py
git commit --amend --no-edit

# 修改作者資訊
git commit --amend --author="Name <email@example.com>"
```

### 取消變更

```bash
# 取消工作目錄的修改（危險！會遺失修改）
git restore file.py
# 或舊語法
git checkout -- file.py

# 取消暫存（從暫存區移除，但保留修改）
git restore --staged file.py
# 或
git reset HEAD file.py

# 取消最後一次 commit（保留變更）
git reset --soft HEAD~1

# 取消最後一次 commit（放棄變更）
git reset --hard HEAD~1

# 取消特定檔案的所有修改
git checkout HEAD -- file.py
```

### Stash（暫存工作）

```bash
# 暫存目前的修改
git stash

# 暫存包含未追蹤的檔案
git stash -u

# 列出所有 stash
git stash list

# 套用最新的 stash
git stash pop

# 套用特定 stash
git stash apply stash@{1}

# 查看 stash 內容
git stash show

# 刪除 stash
git stash drop stash@{0}

# 清空所有 stash
git stash clear
```

### Rebase（變基）

```bash
# 將 feature 分支 rebase 到 main
git checkout feature-branch
git rebase main

# 互動式 rebase（修改歷史）
git rebase -i HEAD~3

# 在 rebase 過程中：
# - pick: 保留 commit
# - reword: 修改 commit 訊息
# - edit: 修改 commit 內容
# - squash: 合併到前一個 commit
# - drop: 刪除 commit

# 取消 rebase
git rebase --abort

# 繼續 rebase（解決衝突後）
git add .
git rebase --continue
```

### Cherry-pick（挑選 commit）

```bash
# 將特定 commit 套用到當前分支
git cherry-pick abc123

# 挑選多個 commit
git cherry-pick abc123 def456

# 只挑選但不 commit
git cherry-pick -n abc123
```

### 標籤（Tag）

```bash
# 建立輕量標籤
git tag v1.0.0

# 建立註解標籤
git tag -a v1.0.0 -m "Release version 1.0.0"

# 列出所有標籤
git tag

# 查看標籤資訊
git show v1.0.0

# 推送標籤到遠端
git push origin v1.0.0

# 推送所有標籤
git push --tags

# 刪除本地標籤
git tag -d v1.0.0

# 刪除遠端標籤
git push origin --delete v1.0.0

# 檢出特定標籤
git checkout v1.0.0
```

---

## 常見問題與解決方案

### 問題 1: 不小心 commit 了敏感資訊

```bash
# 方法 1: 如果還沒 push
git reset --soft HEAD~1
# 編輯檔案，移除敏感資訊
git add .
git commit -m "Fix: Remove sensitive data"

# 方法 2: 已經 push（需要改寫歷史，危險！）
# 1. 從歷史中移除檔案
git filter-branch --tree-filter 'rm -f config/secrets.json' HEAD
# 2. 強制推送
git push --force

# 方法 3: 使用 BFG Repo-Cleaner（推薦）
# 下載 BFG: https://rtyley.github.io/bfg-repo-cleaner/
java -jar bfg.jar --delete-files secrets.json
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push --force
```

### 問題 2: 合併衝突

```bash
# 當 git merge 或 git pull 出現衝突時：
# 1. 查看衝突檔案
git status

# 2. 編輯檔案，解決衝突標記
# <<<<<<< HEAD
# 你的變更
# =======
# 對方的變更
# >>>>>>> branch-name

# 3. 標記為已解決
git add conflicted-file.py

# 4. 完成合併
git commit

# 如果想放棄合併
git merge --abort
```

### 問題 3: 推送被拒絕

```bash
# 錯誤訊息：
# ! [rejected] main -> main (fetch first)

# 原因：遠端有新的 commit

# 解決方法 1: 先 pull 再 push
git pull
git push

# 解決方法 2: rebase
git pull --rebase
git push

# 強制推送（危險！會覆蓋遠端）
git push --force
# 或更安全的版本（只在沒人改動時才推送）
git push --force-with-lease
```

### 問題 4: 想恢復已刪除的檔案

```bash
# 從最後一次 commit 恢復
git restore file.py

# 從特定 commit 恢復
git checkout abc123 -- file.py

# 查找刪除檔案的 commit
git log --all --full-history -- file.py

# 恢復整個目錄
git checkout abc123 -- path/to/directory/
```

### 問題 5: Commit 到錯誤的分支

```bash
# 情境：在 main 上 commit 了，但應該在 feature 分支

# 1. 記住 commit hash
git log -1
# commit abc123...

# 2. 回到 commit 前的狀態
git reset --hard HEAD~1

# 3. 切換到正確的分支
git checkout feature-branch

# 4. 套用那個 commit
git cherry-pick abc123
```

---

## 最佳實踐

### Commit 訊息規範

**基本格式：**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type 類型：**
- `feat`: 新功能
- `fix`: 修正 bug
- `docs`: 文件更新
- `style`: 格式調整（不影響程式碼運行）
- `refactor`: 重構
- `test`: 測試相關
- `chore`: 建置或輔助工具變動

**範例：**
```bash
git commit -m "feat(auth): add login functionality"

git commit -m "fix(api): resolve null pointer exception in user service"

git commit -m "docs: update installation guide"

# 多行訊息
git commit -m "feat(ui): add dark mode

- Add dark mode toggle in settings
- Update all components to support theming
- Add theme persistence to localStorage

Closes #123"
```

### .gitignore 最佳實踐

```gitignore
# 1. 從通用到特定
# Python
*.pyc
__pycache__/

# 環境
.venv/
.env

# IDE
.vscode/
.idea/

# 專案特定
data/raw/*
!data/raw/.gitkeep

# 2. 使用註解說明
# Build artifacts (generated by setup.py)
dist/
build/

# 3. 使用否定模式保留特定檔案
logs/*
!logs/.gitkeep
!logs/README.md
```

### 分支命名規範

```bash
# 功能分支
feature/user-authentication
feature/payment-integration

# 修復分支
fix/login-bug
hotfix/critical-security-patch

# 文件分支
docs/api-documentation

# 重構分支
refactor/database-optimization

# 版本分支
release/v1.2.0
```

### 定期維護

```bash
# 清理已合併的本地分支
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# 清理遠端追蹤分支（遠端已刪除）
git fetch --prune

# 清理不需要的檔案和優化倉庫
git gc --aggressive --prune=now

# 查看倉庫大小
git count-objects -vH
```

---

## Git 工作流程模式

### 1. Feature Branch Workflow

```bash
# 1. 更新 main
git checkout main
git pull

# 2. 建立 feature 分支
git checkout -b feature/new-feature

# 3. 開發
git add .
git commit -m "feat: implement new feature"

# 4. 推送
git push -u origin feature/new-feature

# 5. 建立 Pull Request
gh pr create --title "Add new feature"

# 6. Review 後合併
# 7. 刪除分支
git checkout main
git pull
git branch -d feature/new-feature
```

### 2. Gitflow Workflow

```bash
# 長期分支
- main (production)
- develop (development)

# 短期分支
- feature/* (新功能)
- release/* (發布準備)
- hotfix/* (緊急修復)

# 範例：
git checkout develop
git checkout -b feature/user-profile
# 開發...
git checkout develop
git merge feature/user-profile
```

### 3. Fork Workflow（開源專案）

```bash
# 1. Fork repository（在 GitHub 上）

# 2. Clone 你的 fork
git clone https://github.com/yourname/project.git

# 3. 新增上游 remote
git remote add upstream https://github.com/original/project.git

# 4. 建立 feature 分支
git checkout -b feature/new-feature

# 5. 開發並 commit
git add .
git commit -m "feat: add new feature"

# 6. 推送到你的 fork
git push origin feature/new-feature

# 7. 建立 Pull Request 到上游

# 8. 保持同步
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## 實用 Git 指令組合

### 每日工作檢查

```bash
# 檢查狀態並查看簡短 log
git status && git log --oneline -5
```

### 快速提交

```bash
# 加入所有變更並提交
git add -A && git commit -m "Your message" && git push
```

### 清理與更新

```bash
# 更新並清理
git pull && git fetch --prune && git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
```

### 查看貢獻統計

```bash
# 查看每個作者的 commit 數
git shortlog -sn

# 查看程式碼行數變更
git log --author="Your Name" --pretty=tformat: --numstat | \
  awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'
```

---

## 學習資源

### 官方文檔
- [Git 官方文檔](https://git-scm.com/doc)
- [Pro Git 書籍（繁體中文）](https://git-scm.com/book/zh-tw/v2)
- [GitHub CLI 文檔](https://cli.github.com/manual/)
- [GitHub 官方指南](https://docs.github.com/)

### 互動式學習
- [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_TW)
- [GitHub Learning Lab](https://lab.github.com/)
- [Git 圖解](https://marklodato.github.io/visual-git-guide/index-zh-tw.html)

### 速查表
- [Git Cheat Sheet（GitHub）](https://education.github.com/git-cheat-sheet-education.pdf)
- [Git 中文速查表](https://github.com/flyhigher139/Git-Cheat-Sheet/blob/master/Git%20Cheat%20Sheet-Zh.md)

---

## 總結

### Git 學習路徑

**初學者：**
1. ✅ 理解基本概念（commit, branch, remote）
2. ✅ 學會基本操作（add, commit, push, pull）
3. ✅ 建立 .gitignore
4. ✅ 撰寫良好的 commit 訊息

**中級：**
1. ✅ 掌握分支操作
2. ✅ 解決合併衝突
3. ✅ 使用 GitHub CLI
4. ✅ 理解 Pull Request 流程

**進階：**
1. ✅ Rebase vs Merge
2. ✅ Cherry-pick 和 Stash
3. ✅ 改寫歷史（慎用）
4. ✅ Git Hooks 和自動化

### 記住這些核心命令

```bash
# 日常 95% 會用到的命令
git status          # 查看狀態（最常用！）
git add .           # 加入所有變更
git commit -m ""    # 提交
git push            # 推送
git pull            # 拉取
git log --oneline   # 查看歷史
git checkout -b     # 建立並切換分支
git merge           # 合併分支
```

**最重要的建議：**
1. 💡 經常 `git status` 確認狀態
2. 💡 常常 commit，每個 commit 做一件事
3. 💡 推送前先 pull
4. 💡 不確定時先建分支
5. 💡 寫清楚的 commit 訊息

---

## 快速參考卡

### Git 命令分類

**基礎操作**
```bash
git init
git clone <url>
git add <file>
git commit -m "message"
git status
git log
```

**分支操作**
```bash
git branch
git checkout <branch>
git merge <branch>
git branch -d <branch>
```

**遠端操作**
```bash
git remote -v
git push
git pull
git fetch
```

**取消操作**
```bash
git restore <file>
git restore --staged <file>
git reset HEAD~1
git revert <commit>
```

**GitHub CLI**
```bash
gh repo create
gh pr create
gh issue create
gh auth login
```

---

恭喜！您現在已經掌握了 Git 和 GitHub CLI 的完整使用方法。

記住：**Git 最好的學習方式就是多用**。從小專案開始練習，遇到問題就查文檔，很快就能熟練掌握！
