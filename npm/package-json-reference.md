# package.json 欄位速查（npm 實務版）

## 1. 最小可用範例

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "private": true
}
```

## 2. 常用欄位速查

| 欄位 | 作用 | 常見值/範例 |
|------|------|-------------|
| `name` | 套件名稱 | `"my-app"` |
| `version` | 版本號（SemVer） | `"1.2.3"` |
| `private` | 防止誤發佈到 npm registry | `true` |
| `type` | 模組系統 | `"module"` 或 `"commonjs"` |
| `scripts` | 專案命令入口 | `"dev": "tsx watch src/index.ts"` |
| `dependencies` | 執行時依賴 | `"express": "^4.19.2"` |
| `devDependencies` | 開發時依賴 | `"typescript": "^5.8.0"` |
| `engines` | Node/npm 版本限制 | `"node": ">=22 <23"` |
| `main` | CommonJS 入口（套件） | `"dist/index.cjs"` |
| `module` | ESM 入口（部分工具會讀） | `"dist/index.js"` |
| `types` | TypeScript 型別入口 | `"dist/index.d.ts"` |
| `exports` | 控制可匯入入口 | 見下方範例 |
| `files` | 發佈時要包含的檔案 | `["dist", "README.md"]` |
| `bin` | CLI 指令映射 | `"mycli": "bin/cli.js"` |
| `repository` | 原始碼位置 | `"github:user/repo"` |
| `license` | 授權 | `"MIT"` |
| `overrides` | 強制覆寫子依賴版本 | `"minimist": "^1.2.8"` |
| `workspaces` | Monorepo 工作區 | `["apps/*", "packages/*"]` |

## 3. 應用程式常用模板（App）

```json
{
  "name": "telegram-bot-app",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "typecheck": "tsc --noEmit",
    "lint": "eslint .",
    "test": "vitest run"
  },
  "dependencies": {
    "dotenv": "^16.4.7"
  },
  "devDependencies": {
    "tsx": "^4.20.0",
    "typescript": "^5.8.0"
  },
  "engines": {
    "node": ">=22 <23",
    "npm": ">=10"
  }
}
```

## 4. 函式庫常用模板（Library）

```json
{
  "name": "@your-scope/my-lib",
  "version": "1.0.0",
  "type": "module",
  "main": "./dist/index.cjs",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  },
  "files": ["dist", "README.md", "LICENSE"],
  "scripts": {
    "build": "tsup src/index.ts --format esm,cjs --dts",
    "test": "vitest run"
  }
}
```

## 5. dependencies vs devDependencies

- 放 `dependencies`：程式在執行環境需要它。
- 放 `devDependencies`：只在開發、建置、測試需要。

簡單判斷：
- 你的 `node dist/index.js` 執行時會 `import` 到的，通常是 `dependencies`。
- 只在 `npm run build/test/lint` 使用到的，通常是 `devDependencies`。

## 6. engines + .nvmrc 建議

建議一起使用：
- `.nvmrc`：給開發者本機快速切換版本。
- `engines`：讓工具與 CI 明確知道版本要求。

範例：

```json
{
  "engines": {
    "node": "22.14.0",
    "npm": "10.x"
  }
}
```

`.nvmrc`：

```text
22.14.0
```

## 7. 發佈相關欄位（要發 package 才需要）

- `private: true`：不發佈專案時建議打開。
- `publishConfig`：指定發佈 registry 或 access。
- `files`：控制打包內容，避免把多餘檔案發出去。
- `bin`：把 JS 檔註冊成命令列工具。

## 8. 常見錯誤與修正

### Q1: `type: module` 後 `require` 不能用？

- `type: module` 代表預設走 ESM。
- 改用 `import`，或把該檔改成 `.cjs`。

### Q2: `npm ci` 失敗但 `npm install` 可以？

- 通常是 `package-lock.json` 與 `package.json` 不一致。
- 解法：先在本機 `npm install` 讓 lock file 同步，再提交兩者。

### Q3: 是否需要 `main`、`module`、`types`？

- 應用程式通常不需要全設。
- 函式庫建議設完整，降低使用者整合問題。

