# 專案結構與配置指南

本資料夾包含各種類型專案的結構設計和配置最佳實踐。

## 📚 文件列表

### ✅ 已完成

| 文件 | 說明 | 字數 |
|------|------|------|
| [python-complete-guide.md](python-complete-guide.md) | Python 專案完整結構指南 | 15,000+ |

### 🔜 規劃中

| 文件 | 說明 | 狀態 |
|------|------|------|
| web-projects.md | Web 專案結構（React, Vue, Node.js） | 規劃中 |
| data-science-projects.md | 資料科學專案結構 | 規劃中 |
| microservices.md | 微服務架構專案結構 | 規劃中 |
| monorepo.md | Monorepo 專案管理 | 規劃中 |

---

## 📖 python-complete-guide.md 內容概覽

完整的 Python 專案結構與配置指南，涵蓋各種專案類型。

### 章節結構

```
1. 專案結構概覽
   ├── 完整專案結構
   ├── 檔案分類
   └── 結構設計原則

2. 核心檔案說明
   ├── README.md
   ├── QUICKSTART.md
   └── CLAUDE.md

3. 文件檔案說明
   ├── 專案文檔
   └── 使用手冊

4. 配置檔案說明
   ├── pyproject.toml
   ├── .gitignore
   ├── requirements.txt
   ├── uv.lock
   └── .editorconfig

5. 環境設定檔案
   ├── .vscode/settings.json
   ├── .vscode/launch.json
   └── .vscode/extensions.json

6. 建立新專案的步驟
   ├── 使用 uv
   └── 手動建立

7. 最佳實踐建議
   ├── 文件撰寫原則
   ├── 版本控制原則
   ├── 依賴管理原則
   └── 目錄結構建議

8. 不同類型專案的結構
   ├── CLI 工具
   ├── Web API
   ├── 資料科學專案
   └── Library 套件

9. 檢查清單
   ├── 必要檔案
   ├── 強烈建議
   └── 依需求新增

10. 常見問題
    └── FAQ
```

---

## 🎯 適用專案類型

### Python 專案

#### 命令列工具（CLI）
```python
cli-tool/
├── src/
│   └── my_cli/
│       ├── cli.py
│       ├── commands/
│       └── utils.py
├── tests/
├── README.md
└── pyproject.toml
```

#### Web API
```python
api-project/
├── src/
│   └── my_api/
│       ├── main.py
│       ├── models/
│       ├── routes/
│       └── services/
├── tests/
├── alembic/
└── docker-compose.yml
```

#### 資料科學
```python
data-science/
├── notebooks/
├── src/
│   └── analysis/
├── data/
│   ├── raw/
│   └── processed/
├── models/
└── reports/
```

#### Python 套件
```python
my-library/
├── src/
│   └── my_lib/
├── tests/
├── docs/
├── examples/
└── LICENSE
```

---

## 🔍 快速查找

### 檔案相關

| 需求 | 章節 |
|------|------|
| 專案需要哪些檔案？ | python-complete-guide.md → 檢查清單 |
| 如何撰寫 README？ | python-complete-guide.md → README.md |
| pyproject.toml 設定？ | python-complete-guide.md → pyproject.toml |
| .gitignore 範本？ | python-complete-guide.md → .gitignore |

### 專案類型

| 專案類型 | 章節 |
|---------|------|
| CLI 工具 | python-complete-guide.md → 命令列工具 |
| Web API | python-complete-guide.md → Web API |
| 資料科學 | python-complete-guide.md → 資料科學專案 |
| 套件開發 | python-complete-guide.md → Library 套件 |

### 環境設定

| 工具 | 章節 |
|------|------|
| VSCode | python-complete-guide.md → .vscode 設定 |
| uv | python-complete-guide.md → 使用 uv |
| EditorConfig | python-complete-guide.md → .editorconfig |

---

## 💡 學習建議

### 1. 了解結構目的
- 理解為什麼需要每個檔案
- 知道什麼時候需要什麼
- 避免過度工程

### 2. 從簡單開始
- 小專案用簡單結構
- 隨專案成長逐步擴展
- 不要一開始就建立所有檔案

### 3. 參考範例
- 研究優秀開源專案
- 了解業界標準
- 建立自己的模板

### 4. 保持一致性
- 團隊內使用統一結構
- 遵循 PEP 8 等規範
- 使用 EditorConfig

---

## 🛠️ 相關工具

### 專案模板工具
- [cookiecutter](https://github.com/cookiecutter/cookiecutter) - 專案模板生成器
- [copier](https://github.com/copier-org/copier) - 專案範本引擎
- [pyscaffold](https://pyscaffold.org/) - Python 專案腳手架

### Python 套件管理
- [uv](https://docs.astral.sh/uv/) - 極快的 Python 套件管理器
- [poetry](https://python-poetry.org/) - 依賴管理和打包
- [pdm](https://pdm.fming.dev/) - 現代化 Python 套件管理

### 程式碼品質工具
- [ruff](https://github.com/astral-sh/ruff) - 極快的 linter
- [black](https://github.com/psf/black) - 程式碼格式化
- [mypy](http://mypy-lang.org/) - 靜態類型檢查
- [pytest](https://pytest.org/) - 測試框架

---

## 📝 最佳實踐要點

### 必做清單 ✅
- [ ] 建立清晰的 README
- [ ] 設定 .gitignore
- [ ] 使用 pyproject.toml
- [ ] 撰寫測試
- [ ] 版本控制

### 強烈建議 📝
- [ ] 加入 QUICKSTART 指南
- [ ] 設定 EditorConfig
- [ ] 使用虛擬環境
- [ ] 撰寫文檔
- [ ] CI/CD 自動化

### 避免事項 ❌
- ❌ 提交虛擬環境到 Git
- ❌ 硬編碼敏感資訊
- ❌ 忽略測試
- ❌ 過度複雜的結構
- ❌ 忘記更新文檔

---

## 📚 延伸閱讀

- [Python Packaging 官方指南](https://packaging.python.org/)
- [PEP 621 - pyproject.toml 規範](https://peps.python.org/pep-0621/)
- [The Hitchhiker's Guide to Python](https://docs.python-guide.org/)
- [Real Python - Project Structure](https://realpython.com/python-application-layouts/)

---

## 🔄 更新記錄

- **2026-01-24** - 建立 python-complete-guide.md

---

## 🤝 貢獻

歡迎分享您的專案結構經驗！

**可以貢獻：**
- 不同領域的專案結構範例
- 配置檔案最佳實踐
- 工具使用技巧
- 常見陷阱和解決方案

---

**開始建立專業專案！** 🚀
