# Firebase Hosting 部署完整教學

本指南帶你從零開始，把前端靜態網站或 React PWA 部署到 Firebase Hosting。

## 目錄

- [1. 架構與目標](#1-架構與目標)
- [2. 前置準備](#2-前置準備)
- [3. 建立 Firebase 專案](#3-建立-firebase-專案)
- [4. 安裝 Firebase CLI 與登入](#4-安裝-firebase-cli-與登入)
- [5. 設定 .firebaserc](#5-設定-firebaserc)
- [6. 確認 firebase.json](#6-確認-firebasejson)
- [7. 環境變數設定](#7-環境變數設定)
- [8. Build 與部署](#8-build-與部署)
- [9. API Key 安全設定](#9-api-key-安全設定)
- [10. 自訂網域（選用）](#10-自訂網域選用)
- [11. 更新部署](#11-更新部署)
- [12. 常見問題排查](#12-常見問題排查)
- [13. 上線前檢查清單](#13-上線前檢查清單)

---

## 1. 架構與目標

Firebase Hosting 是 Google 提供的靜態網站托管服務，提供全球 CDN、自動 HTTPS、自訂網域，免費額度對個人專案非常夠用。

目標流程：

```
本機開發 → npm run build → dist/ → firebase deploy → https://你的專案.web.app
```

適合場景：
- React / Vue / Svelte 等前端 SPA
- PWA（Progressive Web App）
- 純靜態 HTML / CSS / JS 網站
- 呼叫外部 API 的純前端 App（如 Gemini、OpenAI）

Firebase Hosting 的特點：

| 特點 | 說明 |
|------|------|
| 自動 HTTPS | 所有部署自動提供 SSL 憑證 |
| 全球 CDN | 靜態資源自動快取加速 |
| SPA 支援 | `rewrites` 設定讓所有路由導向 `index.html` |
| 免費額度 | 每月 10GB 流量、1GB 儲存（Spark 免費方案） |
| 部署速度 | 通常 30 秒內完成 |

---

## 2. 前置準備

你需要：
- Node.js 18+（建議 LTS 版本）
- npm（隨 Node.js 附帶）
- Google 帳號
- 你的前端專案（本指南以 Vite + React 為範例）

確認環境：

```bash
node --version
npm --version
```

確認專案可以在本機正常 build：

```bash
npm run build
# 應產生 dist/ 資料夾
```

---

## 3. 建立 Firebase 專案

### 3.1 建立專案

1. 前往 [Firebase Console](https://console.firebase.google.com)
2. 點「**新增專案**」
3. 輸入專案名稱（例如 `labelsense`）
4. Firebase Console 會自動產生專案 ID（例如 `labelsense-9d314`）
5. **Google Analytics**：可關閉（靜態網站不需要）
6. **Gemini in Firebase AI 輔助功能**：按「繼續」跳過即可
7. 等待建立完成

> **記下你的專案 ID**，後續步驟會用到（格式通常是 `專案名-xxxxx`）。

### 3.2 確認專案 ID

在 Firebase Console 右上角的齒輪圖示 → **專案設定**，可找到「專案 ID」。

或用 CLI 查詢（安裝 CLI 後）：

```bash
firebase projects:list
```

---

## 4. 安裝 Firebase CLI 與登入

### 4.1 安裝

```bash
npm install -g firebase-tools
```

確認安裝成功：

```bash
firebase --version
# 例如：13.x.x
```

### 4.2 登入

```bash
firebase login
```

執行後會自動開啟瀏覽器，用你建立 Firebase 專案的 Google 帳號授權。

回到終端機看到以下訊息代表成功：

```
✔  Success! Logged in as yourname@gmail.com
```

### 4.3 登出（需要切換帳號時）

```bash
firebase logout
firebase login
```

---

## 5. 設定 .firebaserc

`.firebaserc` 告訴 Firebase CLI 要部署到哪個專案。

檔案位置：專案根目錄 `.firebaserc`

```json
{
  "projects": {
    "default": "你的專案ID"
  }
}
```

**範例**（專案 ID 為 `labelsense-9d314`）：

```json
{
  "projects": {
    "default": "labelsense-9d314"
  }
}
```

> 如果你是第一次設定，可以用 `firebase init hosting` 讓 CLI 自動產生（見下方說明），或手動建立此檔案。

### 用 CLI 初始化（全新專案用）

如果你的專案**還沒有** `firebase.json` 與 `.firebaserc`：

```bash
firebase init hosting
```

依序回答問題：

```
? Please select an option: Use an existing project
? Select a default Firebase project: labelsense-9d314
? What do you want to use as your public directory? dist
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
? Set up automatic builds and deploys with GitHub? No
```

CLI 會自動建立 `firebase.json` 與 `.firebaserc`。

---

## 6. 確認 firebase.json

`firebase.json` 控制 Hosting 的行為。以下是 React SPA / PWA 的建議設定：

```json
{
  "hosting": {
    "public": "dist",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ],
    "headers": [
      {
        "source": "**/*.@(js|css)",
        "headers": [
          {
            "key": "Cache-Control",
            "value": "public, max-age=31536000, immutable"
          }
        ]
      },
      {
        "source": "**",
        "headers": [
          {
            "key": "X-Frame-Options",
            "value": "SAMEORIGIN"
          },
          {
            "key": "X-Content-Type-Options",
            "value": "nosniff"
          }
        ]
      }
    ]
  }
}
```

各欄位說明：

| 欄位 | 說明 |
|------|------|
| `public` | 要部署的資料夾，Vite 預設輸出到 `dist` |
| `ignore` | 不上傳的檔案 |
| `rewrites` | 所有路徑都導向 `index.html`（SPA 必要設定） |
| `headers` | JS/CSS 設定長期快取；全站加入安全標頭 |

---

## 7. 環境變數設定

### Vite 專案

Vite 專案的環境變數必須以 `VITE_` 開頭才能在前端使用。

建立 `.env` 檔案（從 `.env.example` 複製）：

```bash
cp .env.example .env
```

`.env` 範例：

```
VITE_GEMINI_API_KEY=AIzaSy...你的key...
VITE_GEMINI_MODEL=gemini-2.0-flash
```

> `.env` 必須加入 `.gitignore`，絕對不要提交到 Git。

確認 `.gitignore` 有這一行：

```
.env
```

### 重要提醒

Vite build 時會把 `VITE_` 開頭的環境變數**直接打包進 JS bundle**，代表：

- 任何人開啟瀏覽器 DevTools 都能看到 Key
- 部署後必須在 Google Cloud Console 設定 API Key 限制（見第 9 章）

---

## 8. Build 與部署

### 8.1 一鍵 Build + Deploy

```bash
npm run build && firebase deploy
```

### 8.2 分步執行

```bash
# 步驟一：打包
npm run build

# 步驟二：部署
firebase deploy
```

### 8.3 成功輸出範例

```
=== Deploying to 'labelsense-9d314'...

i  deploying hosting
i  hosting[labelsense-9d314]: beginning deploy...
i  hosting[labelsense-9d314]: found 12 files in dist
✔  hosting[labelsense-9d314]: file upload complete
i  hosting[labelsense-9d314]: finalizing version...
✔  hosting[labelsense-9d314]: version finalized
i  hosting[labelsense-9d314]: releasing new version...
✔  hosting[labelsense-9d314]: release complete

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/labelsense-9d314/overview
Hosting URL: https://labelsense-9d314.web.app
```

### 8.4 只部署 Hosting（多服務專案用）

若專案同時用了 Firebase 其他服務（Firestore、Functions 等），可指定只部署 Hosting：

```bash
firebase deploy --only hosting
```

---

## 9. API Key 安全設定

因為 API Key 會打包進前端 bundle，**部署後必須設定限制**，否則 Key 可能被任何人盜用。

### 9.1 前往 Google Cloud Console

1. 開啟 [Google Cloud Console > 憑證](https://console.cloud.google.com/apis/credentials)
2. 在憑證列表找到你的 API Key
3. 點選 Key 名稱進入編輯頁面

### 9.2 設定 HTTP 參照網址限制

1. 「**應用程式限制**」區塊 → 選「**HTTP 參照網址（網站）**」
2. 點「新增項目」，加入以下兩行：

```
https://你的專案ID.web.app/*
https://你的專案ID.firebaseapp.com/*
```

範例：

```
https://labelsense-9d314.web.app/*
https://labelsense-9d314.firebaseapp.com/*
```

3. 如果有自訂網域也要加入：

```
https://你的自訂網域/*
```

4. 點「**儲存**」

> 設定後，只有從這些網址發出的請求才能使用此 Key，即使 Key 外洩也無法從其他網站呼叫。

---

## 10. 自訂網域（選用）

Firebase Hosting 預設網址是 `https://專案ID.web.app`，如果你有自己的網域可以綁定。

### 10.1 在 Firebase Console 設定

1. Firebase Console → **Hosting** → **新增自訂網域**
2. 輸入你的網域（例如 `labelsense.example.com`）
3. Firebase 會給你兩組 DNS 記錄需要設定

### 10.2 在你的 DNS 服務商設定

依 Firebase 提示，在 DNS 服務商（Cloudflare、GoDaddy 等）新增：

```
類型: A
名稱: @（或你的子網域）
內容: Firebase 給的 IP
```

### 10.3 等待生效

DNS 傳播最多需要 24 小時，Firebase 會自動核發 SSL 憑證。

---

## 11. 更新部署

每次修改 code 後，重新 build + deploy 即可：

```bash
npm run build && firebase deploy
```

Firebase Hosting 有版本歷史，可以在 Console 查看並**一鍵回滾**到舊版本：

Firebase Console → **Hosting** → **Release history** → 找到舊版本 → 右側選單 → **Rollback**

---

## 12. 常見問題排查

### firebase: command not found

原因：全域安裝沒有加入 PATH。

```bash
# 方法一：重新開終端機
# 方法二：用 npx 執行
npx firebase-tools deploy
```

### Error: Failed to get Firebase project

原因：登入帳號與專案不符，或尚未登入。

```bash
firebase login --reauth
firebase projects:list  # 確認看得到你的專案
```

### 部署成功但網頁顯示空白

可能原因：

1. `.env` 沒有設定 API Key
2. Vite build 時環境變數沒有讀到（確認 `.env` 在專案根目錄）
3. 開啟瀏覽器 DevTools → Console 看錯誤訊息

### SPA 頁面重新整理後 404

原因：`firebase.json` 的 `rewrites` 沒有設定。

確認 `firebase.json` 有以下設定：

```json
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
```

### 條碼掃描或相機功能無法使用

原因：瀏覽器要求 HTTPS 才能存取相機。Firebase Hosting 已自動提供 HTTPS，應可正常運作。

若仍無法使用：
- 確認瀏覽器已授予相機權限
- iOS Safari 請確認在「設定 → Safari → 相機」中已允許

### API 回傳 403 或 referer not allowed

原因：Google Cloud Console 的 API Key 已設定 HTTP 參照限制，但你的網域不在白名單中。

解法：回到第 9 章，確認 `https://你的專案ID.web.app/*` 已加入。

---

## 13. 上線前檢查清單

```
□ firebase.json 的 public 指向正確的 build 輸出資料夾（Vite 為 dist）
□ .firebaserc 的 default 填入正確的 Firebase 專案 ID
□ .env 已設定所有必要環境變數
□ .env 已加入 .gitignore（確認沒有被 commit）
□ npm run build 本機執行無錯誤
□ firebase deploy 成功，Hosting URL 可正常開啟
□ 手機瀏覽器實機測試（PWA 安裝、相機、深色模式）
□ Google Cloud Console 已設定 API Key HTTP 參照限制
□ 如有自訂網域，DNS 已設定且 SSL 憑證已核發
```
