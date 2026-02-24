# 全專案目錄導覽 + 建議閱讀順序（精簡版）

## 1) 目錄導覽（你會學到什麼）

- `git-github/`：Git 與 GitHub CLI，版本控制與協作流程。
- `project-structure/`：Python 專案工程化結構與配置。
- `python/`：Python 環境管理（目前主軸是 `uv`）。
- `npm/`：Node.js 專案依賴管理、scripts 與團隊協作。
- `docker/`：Docker 入門到進階、Compose、CI/CD。
- `powershell/`：PowerShell 操作、自動化、檔案管理實戰。
- `google-cloud/`：Cloud Run 部署 Python Streamlit。
- `mcp/`：MCP 基本介紹 + Codex 接 Notion MCP。
- `tailscale/`：零信任私網、節點連線、ACL、Exit Node、Subnet Router。
- `railway/`：Python 服務 Railway 雲端部署、Webhook 設定、環境變數管理。

---

## 2) 依程度閱讀順序

## A. 初學者（先建立基本功）

1. `git-github/complete-guide.md`
2. `python/uv-guide.md`
3. `project-structure/python-complete-guide.md`
4. `npm/complete-guide.md`
5. `docker/complete-guide.md`（先讀前半）
6. `powershell/complete-guide.md`（先讀基本命令與檔案操作）

適合你如果：
- 剛開始做專案，想先把開發流程跑順。

---

## B. 中階（從會用到會部署）

1. `docker/complete-guide.md`（完整）
2. `google-cloud/complete-guide.md`
3. `railway/complete-guide.md`
4. `npm/complete-guide.md`
5. `powershell/complete-guide.md`（檔案管理與排程）
6. `tailscale/complete-guide.md`

適合你如果：
- 已會寫程式，想把服務穩定部署與管理。

---

## C. 進階實戰（安全、整合、自動化）

1. `mcp/complete-guide.md`
2. `tailscale/complete-guide.md`（ACL、Exit Node、Subnet Router）
3. `google-cloud/complete-guide.md`（更新、回滾、維運）
4. `docker/complete-guide.md`（安全與最佳化章節）
5. `railway/complete-guide.md`（Webhook 部署、費用管控）

適合你如果：
- 要做團隊協作、跨工具整合、權限治理。

---

## 3) 速讀版本（時間少就照這個）

1. `git-github/complete-guide.md`
2. `python/uv-guide.md`
3. `npm/complete-guide.md`
4. `docker/complete-guide.md`
5. `google-cloud/complete-guide.md`
6. `railway/complete-guide.md`
7. `mcp/complete-guide.md`

---

## 4) 推薦節奏（每週）

- 第 1 週：Git + uv + 專案結構 + npm
- 第 2 週：Docker + PowerShell
- 第 3 週：Cloud Run + Railway + Tailscale
- 第 4 週：MCP + 整體回顧與實作
