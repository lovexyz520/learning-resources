# 程式開發學習資源

這個 repository 收錄實作導向的教學文件，主題包含 Git/GitHub、專案結構、Python 與 Docker。

## 專案結構

```text
learning-resources/
├── git-github/           # Git 與 GitHub 教學
├── project-structure/    # 專案結構與工程化實務
├── python/               # Python 與 uv 環境管理
├── docker/               # Docker 入門到進階實戰
└── README.md
```

---

## 主題文件

### 1. Git 與 GitHub

- 目錄：[`git-github/`](git-github/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`git-github/complete-guide.md`](git-github/complete-guide.md) | Git + GitHub CLI 完整教學 | 已完成 |
| `git-github/quick-reference.md` | 常用指令速查 | 規劃中 |
| `git-github/troubleshooting.md` | 問題排查與修復 | 規劃中 |

### 2. 專案結構

- 目錄：[`project-structure/`](project-structure/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`project-structure/python-complete-guide.md`](project-structure/python-complete-guide.md) | Python 專案結構完整教學 | 已完成 |
| `project-structure/web-projects.md` | Web 專案結構指南 | 規劃中 |
| `project-structure/data-science-projects.md` | 資料科學專案結構指南 | 規劃中 |

### 3. Python

- 目錄：[`python/`](python/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`python/uv-guide.md`](python/uv-guide.md) | uv 完整教學與環境管理 | 已完成 |
| `python/python-basics.md` | Python 基礎語法 | 規劃中 |
| `python/oop.md` | 物件導向基礎 | 規劃中 |

### 4. Docker

- 目錄：[`docker/`](docker/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`docker/complete-guide.md`](docker/complete-guide.md) | Docker 從入門到進階實戰（含 Compose、CI/CD） | 已完成 |
| [`docker/README.md`](docker/README.md) | Docker 主題索引與閱讀建議 | 已完成 |

---

## 快速開始

### 想先學 Git 與 GitHub

1. 讀 [`git-github/complete-guide.md`](git-github/complete-guide.md)
2. 先完成「基本觀念、版本控制流程、分支操作」
3. 再進入「GitHub CLI、PR 與協作流程」

### 想把 Python 專案做得更工程化

1. 讀 [`project-structure/python-complete-guide.md`](project-structure/python-complete-guide.md)
2. 跟著建立標準專案骨架
3. 對照你的現有專案逐步重構

### 想快速上手 Docker 部署

1. 讀 [`docker/complete-guide.md`](docker/complete-guide.md) 第 1 到 6 章
2. 完成 Nginx 與 Dockerfile 範例
3. 再讀 Compose、最佳化、安全與 CI/CD 章節

---

## 常見問題導覽

| 問題 | 參考文件 |
|------|----------|
| 我該怎麼建立 Git 基本工作流？ | [`git-github/complete-guide.md`](git-github/complete-guide.md) |
| Python 專案應該有哪些必要檔案？ | [`project-structure/python-complete-guide.md`](project-structure/python-complete-guide.md) |
| 如何用 uv 管理 Python 環境？ | [`python/uv-guide.md`](python/uv-guide.md) |
| 如何建立 Dockerfile 並啟動容器？ | [`docker/complete-guide.md`](docker/complete-guide.md) |
| 多服務要怎麼用 Compose 管理？ | [`docker/complete-guide.md`](docker/complete-guide.md) |
| Docker 上線前該注意哪些安全事項？ | [`docker/complete-guide.md`](docker/complete-guide.md) |

---

## 學習路線建議

### 初學者（1-2 週）

1. Git 基礎與提交流程
2. Python 專案基礎結構
3. Docker 核心概念與容器操作

### 進階者（3-4 週）

1. 分支策略、PR 協作與版本管理
2. Python 專案工程化與開發工具整合
3. Docker Compose、多服務與排錯流程

### 實戰者（5 週以上）

1. Dockerfile 最佳化與映像安全
2. CI/CD 自動建置與發佈流程
3. Docker 到 Kubernetes 的銜接規劃

---

## 目前進度

| 分類 | 已完成文件數 | 狀態 |
|------|--------------|------|
| Git & GitHub | 1 | 已上線 |
| 專案結構 | 1 | 已上線 |
| Python | 1 | 已上線 |
| Docker | 2 | 已上線 |
| **總計** | **5** | **持續擴充** |

---

## 貢獻方式

1. 建立分支：`git checkout -b feature/new-guide`
2. 新增或更新文件
3. 提交變更：`git commit -m "docs: add/update guide"`
4. 推送分支並發 PR

---

## 聯絡

- GitHub: [@lovexyz520](https://github.com/lovexyz520)
- Email: lovexyz520@gmail.com
