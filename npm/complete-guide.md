# npm 完整教學（從新專案到團隊協作）

## 目錄

1. npm 是什麼
2. 安裝與版本確認
3. 建立第一個 npm 專案
4. 專案核心檔案說明
5. 安裝套件與依賴分類
6. 版本號與 SemVer 規則
7. scripts 與執行流程
8. npx 與一次性工具執行
9. 團隊協作最佳實踐
10. Node 版本管理（nvm/fnm/Volta）
11. 發佈與私有套件基礎
12. 安全、稽核與維運
13. Workspaces（Monorepo）基礎
14. 常見問題排查
15. 實戰練習任務
16. 總結

---

## 1. npm 是什麼

- `npm` 是 Node.js 的套件管理器（Node Package Manager）。
- 在日常開發中，npm 主要做三件事：
  - 管理依賴套件安裝與版本。
  - 管理專案腳本（`npm run ...`）。
  - 管理可重現建置（透過 lock file）。

你可以把 npm 專案理解成：
- `package.json`：需求清單與專案腳本。
- `package-lock.json`：實際安裝版本快照。
- `node_modules/`：本機安裝結果。

---

## 2. 安裝與版本確認

### 2.1 確認 Node 與 npm

```bash
node -v
npm -v
```

### 2.2 建議版本原則

- 盡量使用 LTS（穩定長期支援）版本的 Node。
- 團隊內統一 Node 主版本，降低相依問題。

---

## 3. 建立第一個 npm 專案

### 3.1 初始化

```bash
mkdir my-npm-app
cd my-npm-app
npm init -y
```

### 3.2 安裝第一個依賴

```bash
npm install axios
npm install -D typescript tsx
```

### 3.3 常見結果

- 新增 `package.json`
- 新增 `package-lock.json`
- 新增 `node_modules/`

---

## 4. 專案核心檔案說明

### 4.1 `package.json`

常見重點欄位：
- `name`、`version`：套件識別與版本。
- `scripts`：可執行指令集合。
- `dependencies`：執行期依賴。
- `devDependencies`：開發期依賴。
- `engines`：限制 Node/npm 版本範圍。

範例：

```json
{
  "name": "my-npm-app",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "tsx src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "engines": {
    "node": ">=22 <23",
    "npm": ">=10"
  }
}
```

### 4.2 `package-lock.json`

- 記錄完整依賴樹的實際版本。
- 團隊協作必須提交到 Git。
- CI 建議搭配 `npm ci` 使用，確保重現一致。

### 4.3 `node_modules/`

- 放實際下載的套件內容。
- 不要提交到 Git（加在 `.gitignore`）。

---

## 5. 安裝套件與依賴分類

### 5.1 一般依賴（執行時需要）

```bash
npm install express
```

### 5.2 開發依賴（只在開發與建置需要）

```bash
npm install -D eslint prettier
```

### 5.3 可選依賴

```bash
npm install --save-optional sharp
```

### 5.4 peerDependencies（套件作者常用）

- 用來聲明「宿主專案應該提供的依賴版本」。
- 常見於 plugin 與 library 場景。

---

## 6. 版本號與 SemVer 規則

常見寫法：
- `1.2.3`：固定版本。
- `^1.2.3`：允許同 major 內更新（`1.x.x`）。
- `~1.2.3`：允許同 minor 內更新（`1.2.x`）。
- `>=1.2.3 <2.0.0`：明確範圍。

建議：
- 應用程式可用 `^`，搭配 lock file。
- 關鍵依賴（如框架核心）可採更保守策略。

---

## 7. scripts 與執行流程

### 7.1 定義常用腳本

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "typecheck": "tsc --noEmit",
    "build": "tsc",
    "start": "node dist/index.js",
    "lint": "eslint .",
    "test": "vitest run"
  }
}
```

### 7.2 執行方式

```bash
npm run dev
npm run build
npm test
```

說明：
- `npm test`、`npm start`、`npm stop` 可省略 `run`。
- 其餘腳本一律用 `npm run <script>`。

---

## 8. npx 與一次性工具執行

- `npx` 可直接執行本地或遠端套件命令。
- 常見用途：初始化模板、臨時工具。

```bash
npx create-vite@latest
npx eslint --init
```

---

## 9. 團隊協作最佳實踐

### 9.1 新環境安裝用 `npm ci`

```bash
npm ci
```

- 嚴格依照 `package-lock.json` 安裝。
- 比 `npm install` 更適合 CI/CD。

### 9.2 提交規則

- 必交：`package.json`、`package-lock.json`
- 不交：`node_modules/`

### 9.3 切換分支後建議

```bash
npm ci
npm run build
npm test
```

---

## 10. Node 版本管理（nvm/fnm/Volta）

npm 只管理套件，不管理 Node 執行版本。建議搭配版本管理工具：

- `nvm`：常見且社群成熟。
- `fnm`：速度快、操作簡潔。
- `Volta`：工具鏈固定效果好。

### 10.1 `.nvmrc`

在專案根目錄建立：

```text
22.14.0
```

使用：

```bash
nvm use
```

### 10.2 `engines` 搭配 `.nvmrc`

- `.nvmrc`：幫開發者快速切版本。
- `engines`：幫工具與 CI 驗證版本要求。

---

## 11. 發佈與私有套件基礎

### 11.1 發佈前檢查

```bash
npm whoami
npm version patch
npm publish --access public
```

### 11.2 私有 registry（簡述）

- 可用 npm 組織、GitHub Packages 或企業私服。
- 透過 `.npmrc` 設定 registry 與 token。

---

## 12. 安全、稽核與維運

### 12.1 檢查漏洞

```bash
npm audit
npm audit fix
```

### 12.2 查看過時套件

```bash
npm outdated
```

### 12.3 強制覆寫子依賴

`package.json` 可使用 `overrides`：

```json
{
  "overrides": {
    "minimist": "^1.2.8"
  }
}
```

---

## 13. Workspaces（Monorepo）基礎

當你有多個子專案時，可用 npm workspaces 統一管理。

```json
{
  "name": "my-monorepo",
  "private": true,
  "workspaces": ["apps/*", "packages/*"]
}
```

常見操作：

```bash
npm install
npm run build -w apps/web
npm run test --workspaces
```

---

## 14. 常見問題排查

### Q1：為什麼別人可跑、我不能跑？

常見原因：
- Node 主版本不同。
- `package-lock.json` 沒同步。
- 使用 `npm install` 導致依賴樹漂移。

建議處理：

```bash
nvm use
npm ci
```

### Q2：`npm install` 很慢怎麼辦？

- 確認網路與 registry 設定。
- 清快取再試一次：

```bash
npm cache verify
npm cache clean --force
```

### Q3：`ERESOLVE` 依賴衝突怎麼解？

- 先看衝突是哪兩個套件版本不相容。
- 調整主依賴版本，避免長期依賴 `--legacy-peer-deps`。

### Q4：`node_modules` 要不要 commit？

- 不要，請提交 lock file 即可。

---

## 15. 實戰練習任務

1. 建立一個 TypeScript 專案，加入 `dev/build/start` scripts。
2. 設定 `engines` 與 `.nvmrc`，並驗證 `nvm use`。
3. 用 `npm ci` 重新安裝並跑測試。
4. 故意升級一個套件版本，觀察 lock file 變更。
5. 使用 `npm audit` 找出風險並修復。

---

## 16. 總結

你可以用這個思路記住 npm：

- 用 `package.json` 管需求與流程。
- 用 `package-lock.json` 管可重現版本。
- 用 `node_modules/` 放實際安裝結果。
- 用 `.nvmrc + engines + npm ci` 建立團隊一致性。

做到以上四點，你的 npm 專案就已經具備工程化的基本管理能力。
