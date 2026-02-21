# 程式開發學習資源

這個 repository 收錄實作導向的教學文件，主題包含 Git/GitHub、專案結構、Python、npm、Docker、PowerShell、Google Cloud、MCP 與 Tailscale。

## 專案結構

```text
learning-resources/
├── LEARNING-PATH.md      # 全專案目錄導覽與閱讀路線圖
├── git-github/           # Git 與 GitHub 教學
├── project-structure/    # 專案結構與工程化實務
├── python/               # Python 與 uv 環境管理
├── npm/                  # npm 套件管理與 Node.js 專案實務
├── docker/               # Docker 入門到進階實戰
├── powershell/           # PowerShell 操作與自動化教學
├── google-cloud/         # Google Cloud 實作與部署教學
├── mcp/                  # MCP 與工具整合教學
├── tailscale/            # Tailscale 零信任網路教學
└── README.md
```

## 閱讀地圖（精簡）

- 建議先讀：[`LEARNING-PATH.md`](LEARNING-PATH.md)

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

### 4. npm

- 目錄：[`npm/`](npm/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`npm/complete-guide.md`](npm/complete-guide.md) | npm 從新專案到團隊協作完整教學 | 已完成 |
| [`npm/README.md`](npm/README.md) | npm 主題索引與閱讀建議 | 已完成 |
| [`npm/package-json-reference.md`](npm/package-json-reference.md) | `package.json` 欄位速查與模板 | 已完成 |

### 5. Docker

- 目錄：[`docker/`](docker/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`docker/complete-guide.md`](docker/complete-guide.md) | Docker 從入門到進階實戰（含 Compose、CI/CD） | 已完成 |
| [`docker/README.md`](docker/README.md) | Docker 主題索引與閱讀建議 | 已完成 |

### 6. PowerShell

- 目錄：[`powershell/`](powershell/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`powershell/complete-guide.md`](powershell/complete-guide.md) | PowerShell 完整操作與檔案管理實戰（學習與教學版） | 已完成 |
| [`powershell/README.md`](powershell/README.md) | PowerShell 主題索引與閱讀建議 | 已完成 |

### 7. Google Cloud

- 目錄：[`google-cloud/`](google-cloud/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`google-cloud/complete-guide.md`](google-cloud/complete-guide.md) | Google Cloud Run 部署 Python Streamlit 教學指引 | 已完成 |
| [`google-cloud/README.md`](google-cloud/README.md) | Google Cloud 主題索引與閱讀建議 | 已完成 |

### 8. MCP

- 目錄：[`mcp/`](mcp/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`mcp/complete-guide.md`](mcp/complete-guide.md) | Codex 安裝 Notion MCP 教學（含 MCP 基本介紹） | 已完成 |
| [`mcp/README.md`](mcp/README.md) | MCP 主題索引與閱讀建議 | 已完成 |

### 9. Tailscale

- 目錄：[`tailscale/`](tailscale/)

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [`tailscale/complete-guide.md`](tailscale/complete-guide.md) | Tailscale 使用教學（入門到實務） | 已完成 |
| [`tailscale/README.md`](tailscale/README.md) | Tailscale 主題索引與閱讀建議 | 已完成 |

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

### 想快速掌握 Node.js/npm 專案管理

1. 讀 [`npm/complete-guide.md`](npm/complete-guide.md) 第 1 到 6 章
2. 建立一個專案並完成 `dependencies/devDependencies/scripts`
3. 加上 `.nvmrc` 與 `engines`，再用 `npm ci` 驗證可重現性

### 想快速上手 Docker 部署

1. 讀 [`docker/complete-guide.md`](docker/complete-guide.md) 第 1 到 6 章
2. 完成 Nginx 與 Dockerfile 範例
3. 再讀 Compose、最佳化、安全與 CI/CD 章節

### 想快速熟悉 PowerShell 實務操作

1. 讀 [`powershell/complete-guide.md`](powershell/complete-guide.md) 第 1 到 6 章
2. 先練習基本命令、檔案操作與物件管線
3. 再做檔案管理實戰（壓縮、批次改名、搜尋清理）與排程自動化

### 想部署 Python Streamlit 到 Google Cloud Run

1. 讀 [`google-cloud/complete-guide.md`](google-cloud/complete-guide.md) 第 1 到 5 章
2. 完成 Docker 本機驗證
3. 再做 Artifact Registry 推送與 Cloud Run 部署

### 想在 Codex 接上 Notion MCP

1. 讀 [`mcp/complete-guide.md`](mcp/complete-guide.md) 的 MCP 基本介紹
2. 依你的環境選 URL 或 stdio 安裝模式
3. 用 `codex mcp list` 與 `codex mcp get notion` 驗證連線

### 想用 Tailscale 建立安全私人網路

1. 讀 [`tailscale/complete-guide.md`](tailscale/complete-guide.md) 第 1 到 6 章
2. 先完成節點加入與連線驗證
3. 再做 Exit Node、Subnet Router 與 ACL 控管

---

## 常見問題導覽

| 問題 | 參考文件 |
|------|----------|
| 我該怎麼建立 Git 基本工作流？ | [`git-github/complete-guide.md`](git-github/complete-guide.md) |
| Python 專案應該有哪些必要檔案？ | [`project-structure/python-complete-guide.md`](project-structure/python-complete-guide.md) |
| 如何用 uv 管理 Python 環境？ | [`python/uv-guide.md`](python/uv-guide.md) |
| 如何用 npm 正確管理依賴與版本？ | [`npm/complete-guide.md`](npm/complete-guide.md) |
| 如何建立 Dockerfile 並啟動容器？ | [`docker/complete-guide.md`](docker/complete-guide.md) |
| 多服務要怎麼用 Compose 管理？ | [`docker/complete-guide.md`](docker/complete-guide.md) |
| Docker 上線前該注意哪些安全事項？ | [`docker/complete-guide.md`](docker/complete-guide.md) |
| Windows 自動化腳本應該怎麼寫？ | [`powershell/complete-guide.md`](powershell/complete-guide.md) |
| PowerShell 如何做排程任務？ | [`powershell/complete-guide.md`](powershell/complete-guide.md) |
| 如何把 Streamlit 部署到 Cloud Run？ | [`google-cloud/complete-guide.md`](google-cloud/complete-guide.md) |
| Cloud Run 服務如何更新與回滾？ | [`google-cloud/complete-guide.md`](google-cloud/complete-guide.md) |
| MCP 是什麼、怎麼接到 Codex？ | [`mcp/complete-guide.md`](mcp/complete-guide.md) |
| Notion MCP 要怎麼安裝與驗證？ | [`mcp/complete-guide.md`](mcp/complete-guide.md) |
| Tailscale 怎麼做多裝置安全連線？ | [`tailscale/complete-guide.md`](tailscale/complete-guide.md) |
| Exit Node 與 Subnet Router 怎麼設定？ | [`tailscale/complete-guide.md`](tailscale/complete-guide.md) |

---

## 學習路線建議

### 初學者（1-2 週）

1. Git 基礎與提交流程
2. Python 專案基礎結構
3. npm 依賴與腳本管理
4. Docker 核心概念與容器操作

### 進階者（3-4 週）

1. 分支策略、PR 協作與版本管理
2. Python 專案工程化與開發工具整合
3. npm 協作流程與版本管理
4. Docker Compose、多服務與排錯流程

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
| npm | 3 | 已上線 |
| Docker | 2 | 已上線 |
| PowerShell | 2 | 已上線 |
| Google Cloud | 2 | 已上線 |
| MCP | 2 | 已上線 |
| Tailscale | 2 | 已上線 |
| **總計** | **16** | **持續擴充** |

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
