# Railway 部署完整教學

本指南帶你從零開始，把 Python Telegram Bot（或任何 Python 服務）部署到 Railway。

## 目錄

- [1. 架構與目標](#1-架構與目標)
- [2. 前置準備](#2-前置準備)
- [3. 建立 Railway 專案](#3-建立-railway-專案)
- [4. 連結 GitHub Repo](#4-連結-github-repo)
- [5. 設定環境變數](#5-設定環境變數)
- [6. 取得 Public URL](#6-取得-public-url)
- [7. Webhook vs Polling 設定](#7-webhook-vs-polling-設定)
- [8. 查看 Logs 與驗證部署](#8-查看-logs-與驗證部署)
- [9. 更新部署](#9-更新部署)
- [10. 暫停與恢復服務](#10-暫停與恢復服務)
- [11. 本機開發與線上並存策略](#11-本機開發與線上並存策略)
- [12. 費用與免費額度](#12-費用與免費額度)
- [13. 常見問題排查](#13-常見問題排查)
- [14. 上線前檢查清單](#14-上線前檢查清單)

---

## 1. 架構與目標

Railway 是一個 PaaS（Platform as a Service）雲端部署平台，讓你不需要自己管 VM 或 Docker，只需推 code 到 GitHub，Railway 自動建置並部署。

目標流程：

```
本機開發 → git push → GitHub → Railway 自動偵測 → 建置 → 部署
```

適合場景：
- Telegram Bot（Webhook 模式）
- API 服務
- 背景工作程序

---

## 2. 前置準備

你需要：
- GitHub 帳號，且你的專案已推上 GitHub
- Railway 帳號（用 GitHub 登入即可）
- 專案根目錄有以下其中一個（Railway 自動偵測）：
  - `pyproject.toml`（uv 專案）
  - `requirements.txt`
  - `Dockerfile`

確認專案已在 GitHub：

```bash
git remote -v
# 應顯示 origin https://github.com/你的帳號/你的repo.git
```

---

## 3. 建立 Railway 專案

1. 開啟 [railway.app](https://railway.app) → 用 GitHub 登入
2. 點右上角 **New Project**
3. 選 **Deploy from GitHub repo**
4. 找到你的 repo → 點 **Deploy Now**

Railway 會自動：
- 偵測語言（Python）
- 使用 nixpacks 建置環境
- 安裝依賴（`uv sync` 或 `pip install`）
- 依 `Procfile` 或 `railway.toml` 啟動服務

---

## 4. 連結 GitHub Repo

Railway 連結後，後續每次 `git push` 到 `main`（或你設定的分支），Railway 會自動重新部署。

查看自動部署設定：
- 服務 → **Settings** → **Source** → 可設定要監聽的分支

手動觸發重新部署：
- 服務 → **Deployments** → 點最新那筆右側的 **Redeploy**

---

## 5. 設定環境變數

Railway 不會讀你本機的 `.env` 檔，所有金鑰與設定必須在 Dashboard 設定。

步驟：
1. 進入你的服務 → 上方點 **Variables**
2. 點 **+ New Variable** 逐一新增
3. 或點 **Raw Editor** 貼上多行變數（格式同 `.env`）

常見變數範例（Telegram Bot）：

```
TELEGRAM_BOT_TOKEN=你的token
GOOGLE_MAPS_API_KEY=你的key
GEMINI_API_KEY=你的key
USE_WEBHOOK=true
LOG_LEVEL=INFO
```

> **注意：** `PORT` 不需要設定，Railway 自動注入。

變數新增後 Railway 會自動重新部署。

---

## 6. 取得 Public URL

Webhook 模式需要一個公開 HTTPS URL 讓 Telegram 推送訊息。

步驟：
1. 服務 → **Settings**
2. 往下找 **Networking** 區塊
3. 點 **Generate Domain**
4. Railway 會生成類似 `https://xxxx.up.railway.app` 的 URL
5. 複製這個 URL

然後回到 **Variables**，新增：

```
WEBHOOK_URL=https://xxxx.up.railway.app
```

填完後 Railway 自動重新部署，Bot 啟動時會呼叫 Telegram API 設定 Webhook。

---

## 7. Webhook vs Polling 設定

Telegram Bot 有兩種接收訊息的方式：

| 模式 | 適合環境 | 說明 |
|------|---------|------|
| Polling | 本機開發 | Bot 主動去 Telegram 拉訊息 |
| Webhook | Railway 線上 | Telegram 主動推訊息到你的 URL |

兩者同時只能用一種。建議用環境變數切換：

**`.env.example`（範本）：**

```
USE_WEBHOOK=false        # 本機用 false
WEBHOOK_URL=             # 本機留空
# PORT 由 Railway 自動注入
```

**Railway Variables：**

```
USE_WEBHOOK=true
WEBHOOK_URL=https://xxxx.up.railway.app
```

**程式判斷邏輯（python-telegram-bot）：**

```python
if USE_WEBHOOK:
    app.run_webhook(
        listen="0.0.0.0",
        port=PORT,
        url_path=f"/webhook/{TOKEN}",
        webhook_url=f"{WEBHOOK_URL}/webhook/{TOKEN}",
    )
else:
    app.run_polling()
```

**程式判斷邏輯（aiogram v3）：**

```python
if USE_WEBHOOK:
    from aiohttp import web
    from aiogram.webhook.aiohttp_server import SimpleRequestHandler, setup_application

    webhook_path = f"/webhook/{token}"
    await bot.set_webhook(f"{WEBHOOK_URL}{webhook_path}")

    app = web.Application()
    SimpleRequestHandler(dispatcher=dp, bot=bot).register(app, path=webhook_path)
    setup_application(app, dp, bot=bot)

    runner = web.AppRunner(app)
    await runner.setup()
    await web.TCPSite(runner, "0.0.0.0", PORT).start()
    await asyncio.Event().wait()
else:
    await dp.start_polling(bot)
```

---

## 8. 查看 Logs 與驗證部署

查看即時 Logs：
1. 服務 → **Deployments**
2. 點最新的部署紀錄
3. 點 **View Logs**

部署成功會看到類似：

```
以 Webhook 模式啟動，URL: https://xxxx.up.railway.app/webhook/<TOKEN>，Port: 8080
```

驗證 Bot 正常：
- 去 Telegram 對 Bot 發一則訊息，確認有回應

---

## 9. 更新部署

每次 push 到 GitHub，Railway 自動重新部署：

```bash
# 修改程式碼後
git add .
git commit -m "feat: 更新功能"
git push
```

Railway 偵測到 push 後：
1. 拉取新 code
2. 重新建置
3. 替換舊版服務（零停機替換）

查看部署歷史：
- 服務 → **Deployments** — 每次部署都有紀錄，可查 Logs、比對差異

---

## 10. 暫停與恢復服務

需要暫停時（例如本機測試、節省費用）：

暫停：
- 服務 → **Settings** → 往下找 **Danger Zone** → **Suspend Service**

恢復：
- 服務 → **Settings** → **Resume Service**

> 暫停後 Webhook 失效，本機可正常使用 Polling 模式測試。

---

## 11. 本機開發與線上並存策略

Telegram 同時只能用一種模式（Webhook 或 Polling），兩者會衝突。

### 策略 A：開發用獨立測試 Bot（推薦）

1. 去 BotFather 建立第二個 bot 專門給本機用：
   ```
   /newbot → MyBot_Dev
   ```
2. 本機 `.env` 用測試 bot 的 token：
   ```
   TELEGRAM_BOT_TOKEN=測試用token
   USE_WEBHOOK=false
   ```
3. Railway 保持正式 token，互不干擾

### 策略 B：測試前暫停 Railway

1. Railway Dashboard → 暫停服務
2. 本機跑 Polling 測試：
   ```bash
   uv run python -m src.main     # Foodie Radar
   uv run python main.py         # EatBot
   ```
3. 測完後 Railway 恢復服務

---

## 12. 費用與免費額度

Railway 計費方式：

| 方案 | 說明 |
|------|------|
| Trial | 免費 $5 credit，用完需升級 |
| Hobby | $5/月，再加用量計費 |
| Pro | $20/月起，團隊用 |

輕量 Telegram Bot 費用估計：

| 項目 | 費用 |
|------|------|
| CPU（閒置為主） | 極低 |
| Memory（~100MB） | 極低 |
| 每月總計 | 通常 < $1，含在 $5 credit 內 |

查看目前費用：
- Dashboard 右上角 → **Usage**

設定費用警示：
- **Account Settings** → **Billing** → 設定 Spend Limit

---

## 13. 常見問題排查

### 問題 1：部署失敗 — Build Error

查看 Build Logs，常見原因：

- `pyproject.toml` 格式錯誤
- 缺少 `uv.lock` 或 `requirements.txt`
- Python 版本不符（在 `pyproject.toml` 指定 `requires-python`）

解法：

```toml
# pyproject.toml
[project]
requires-python = ">=3.11"
```

### 問題 2：Bot 啟動但沒反應

檢查項目：
- Variables 是否有 `TELEGRAM_BOT_TOKEN`
- `USE_WEBHOOK=true` 且 `WEBHOOK_URL` 是否正確
- Logs 是否有 Webhook 設定成功的訊息

手動確認 Webhook 狀態（在瀏覽器打開）：

```
https://api.telegram.org/bot<TOKEN>/getWebhookInfo
```

應看到：
```json
{"ok":true,"result":{"url":"https://xxxx.up.railway.app/webhook/<TOKEN>",...}}
```

### 問題 3：PORT 衝突或服務無法啟動

確認 `Procfile` 使用 `web:` 而非 `worker:`：

```
web: uv run python -m src.main
```

`worker:` 是背景工作程序，Railway 不會分配 HTTP 流量給它；`web:` 才會取得 Public URL 與 PORT。

### 問題 4：環境變數沒生效

變數設定後 Railway 會自動重新部署，但若沒有，手動觸發：
- **Deployments** → **Redeploy**

### 問題 5：本機 Polling 拉不到訊息

原因：Railway 的 Webhook 還在，Telegram 把訊息送到 Railway，本機拉不到。

解法：
- 暫停 Railway 服務，再本機啟動
- 或使用獨立測試 Bot（見第 11 章）

---

## 14. 上線前檢查清單

- [ ] `Procfile` 使用 `web:` 並已推上 GitHub
- [ ] `railway.toml` 設定正確的 `startCommand`
- [ ] Railway Variables 填入所有必要金鑰
- [ ] `USE_WEBHOOK=true` 且 `WEBHOOK_URL` 已填入正確 URL
- [ ] Railway 已 Generate Domain
- [ ] 部署 Logs 顯示 Webhook 模式啟動成功
- [ ] Telegram 傳訊息有正常回應
- [ ] 已設定 Billing 費用上限（可選但建議）
- [ ] 本機測試使用獨立測試 Bot，不影響線上服務
