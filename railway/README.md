# Railway 教學資源

本資料夾收錄 Railway 雲端部署主題文件，聚焦在 Python 服務（Telegram Bot）的實際部署流程。

---

## 主要文件

| 檔案 | 說明 | 狀態 |
|------|------|------|
| [complete-guide.md](complete-guide.md) | Railway 部署 Python 服務完整教學 | 已完成 |

---

## 文件重點

- Railway 專案建立與 GitHub 連結
- 環境變數設定（API 金鑰、Webhook 設定）
- Public URL 生成與 Webhook 設定
- Webhook vs Polling 切換策略
- 查看 Logs 與驗證部署
- 自動部署（push → redeploy）流程
- 暫停/恢復服務
- 本機開發與線上並存策略（測試 Bot）
- 費用估計與免費額度說明
- 常見問題排查與上線檢查清單

---

## 建議閱讀順序

1. 先讀 `complete-guide.md` 第 1 到 6 章（建立專案、設定變數、取得 URL）
2. 再讀第 7 章（Webhook vs Polling 程式碼設定）
3. 完成部署後讀第 11 章（本機開發策略）
4. 日常維護參考第 9、10、12、13 章
