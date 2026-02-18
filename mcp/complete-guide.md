# Codex 安裝 Notion MCP 教學（含 MCP 基本介紹）

本指南分成兩部分：
1. MCP 基本介紹（先懂概念）
2. 用 Codex 接上 Notion MCP（可直接操作）

---

## 1. MCP 基本介紹

## 1.1 MCP 是什麼

MCP（Model Context Protocol）是一個讓 AI 模型可以「安全、標準化」連接外部工具與資料源的協定。  
你可以把它理解成：AI 與外部系統之間的統一介面層。

## 1.2 為什麼需要 MCP

沒有 MCP 時：
- 每個工具都要客製整合
- 權限、認證、傳輸方式不一致
- 維護成本高

有 MCP 時：
- 統一工具註冊方式
- 統一認證流程（例如 OAuth / token）
- 工具能力可被模型穩定呼叫

## 1.3 你會遇到的 MCP 名詞

- MCP Server：提供能力的一端（例如 Notion 讀寫）
- MCP Client：呼叫能力的一端（這裡是 Codex）
- Tool：可被呼叫的具體操作（例如 fetch page）
- Transport：
  - `stdio`：本機程序透過標準輸入輸出連接
  - `streamable HTTP`：用 URL 連接遠端 MCP 服務

---

## 2. Codex MCP 指令先看懂

先用這些命令確認版本與語法：

```bash
codex mcp --help
codex mcp add --help
codex mcp login --help
codex mcp list
```

你會看到核心子命令：
- `add`：新增 MCP server 設定
- `list`：列出已設定 server
- `get`：看某個 server 詳細設定
- `login` / `logout`：OAuth 認證管理
- `remove`：移除設定

---

## 3. 安裝 Notion MCP（Cloud URL 方式）

這種方式適用「Notion MCP 有提供可連線 URL」的情境。

## 3.1 新增 Notion MCP server

```bash
codex mcp add notion --url <NOTION_MCP_SERVER_URL>
```

如果該服務需要 Bearer Token（而非 OAuth）：

```bash
codex mcp add notion --url <NOTION_MCP_SERVER_URL> --bearer-token-env-var NOTION_MCP_TOKEN
```

然後在系統環境變數設定 `NOTION_MCP_TOKEN`。

## 3.2 OAuth 登入（若服務支援）

```bash
codex mcp login notion
```

若要指定 scope：

```bash
codex mcp login notion --scopes scope1,scope2
```

## 3.3 驗證設定

```bash
codex mcp list
codex mcp get notion
```

---

## 4. 安裝 Notion MCP（stdio 本機程序方式）

這種方式適用「你本機有一個可啟動的 Notion MCP server 程式」。

```bash
codex mcp add notion --env NOTION_TOKEN=<YOUR_TOKEN> -- <YOUR_NOTION_MCP_COMMAND>
```

範例概念：
- `--env`：注入 server 需要的環境變數
- `-- <COMMAND>`：後面是啟動 MCP server 的實際命令

新增後一樣檢查：

```bash
codex mcp list
codex mcp get notion
```

---

## 5. 實務建議（重要）

## 5.1 命名建議

- 名稱固定用 `notion`，避免團隊內多種名稱造成混亂。

## 5.2 認證建議

- 優先使用 OAuth（若供應端支援）
- API token 不要硬寫進指令歷史，改用環境變數

## 5.3 安全建議

- 僅給必要 Notion 權限（最小權限）
- 定期輪替 token
- 共用機器避免把 token 留在全域 shell 歷史

---

## 6. 驗證是否可在 Codex 中使用 Notion

完成安裝後，你應該能在 Codex 工具層看到 Notion 可用工具（例如 fetch/search/create-page 等能力）。  
若你看到工具呼叫可成功回傳頁面，就代表接入完成。

---

## 7. 常見問題排查

## Q1：`codex mcp list` 沒看到 notion

先重跑：

```bash
codex mcp add notion --url <NOTION_MCP_SERVER_URL>
codex mcp list
```

## Q2：有設定但呼叫失敗

檢查：
- URL 是否正確
- token 是否過期
- OAuth scope 是否不足
- Notion 端頁面權限是否分享給該整合

## Q3：本機 stdio 模式啟動不了

檢查：
- `<YOUR_NOTION_MCP_COMMAND>` 在終端機可獨立啟動
- 需要的環境變數是否完整
- 路徑與執行權限是否正確

---

## 8. 快速抄表

```bash
# 1) 看語法
codex mcp --help

# 2) 新增 Notion（URL 模式）
codex mcp add notion --url <NOTION_MCP_SERVER_URL>

# 3) OAuth 登入（若支援）
codex mcp login notion

# 4) 驗證
codex mcp list
codex mcp get notion
```

