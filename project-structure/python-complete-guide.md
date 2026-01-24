# 完整專案結構與配置指南

本文件說明如何使用 VSCode、uv、Claude Code 和 Git 建立一個專業的 Python 專案。

## 目錄

- [專案結構概覽](#專案結構概覽)
- [核心檔案說明](#核心檔案說明)
- [文件檔案說明](#文件檔案說明)
- [配置檔案說明](#配置檔案說明)
- [環境設定檔案](#環境設定檔案)
- [建立新專案的步驟](#建立新專案的步驟)
- [最佳實踐建議](#最佳實踐建議)
- [不同類型專案的結構](#不同類型專案的結構)

---

## 專案結構概覽

一個完整的 Python 專案通常包含以下檔案結構：

```
my-project/
├── 📄 程式碼檔案
│   ├── main.py                    # 主程式進入點
│   ├── module1.py                 # 功能模組 1
│   └── module2.py                 # 功能模組 2
│
├── 📚 文件檔案
│   ├── README.md                  # 專案說明（必要）
│   ├── QUICKSTART.md             # 快速開始指南（建議）
│   ├── CLAUDE.md                 # Claude Code 指引（使用 Claude Code 時）
│   ├── CONTRIBUTING.md           # 貢獻指南（開源專案）
│   ├── CHANGELOG.md              # 版本更新記錄（建議）
│   └── LICENSE                   # 授權條款（開源專案必要）
│
├── ⚙️ 配置檔案
│   ├── pyproject.toml            # Python 專案配置（必要）
│   ├── uv.lock                   # 依賴版本鎖定（uv 自動生成）
│   ├── requirements.txt          # 依賴清單（傳統方式）
│   ├── .gitignore                # Git 忽略規則（必要）
│   ├── .gitattributes            # Git 屬性設定（可選）
│   └── .editorconfig             # 編輯器配置（建議）
│
├── 🛠️ 開發工具配置
│   ├── .vscode/                  # VSCode 專案設定
│   │   ├── settings.json         # 編輯器設定
│   │   ├── launch.json           # 除錯配置
│   │   └── extensions.json       # 推薦擴充套件
│   ├── .claude/                  # Claude Code 設定
│   │   └── settings.local.json   # 本地設定
│   └── .github/                  # GitHub 相關（開源專案）
│       ├── workflows/            # CI/CD 工作流程
│       └── ISSUE_TEMPLATE/       # Issue 模板
│
├── 📦 虛擬環境（不提交到 Git）
│   └── .venv/                    # Python 虛擬環境
│
├── 🧪 測試檔案（如果有測試）
│   ├── tests/                    # 測試資料夾
│   │   ├── __init__.py
│   │   ├── test_module1.py
│   │   └── test_module2.py
│   └── pytest.ini                # pytest 配置
│
└── 📊 資料與資源（依需求）
    ├── data/                     # 資料檔案
    ├── docs/                     # 詳細文檔
    └── examples/                 # 範例程式
```

---

## 核心檔案說明

### 1. README.md（必要）

**目的：** 專案的門面，第一個被閱讀的文件

**必須包含的內容：**
```markdown
# 專案名稱

簡短描述（1-2 句話說明專案用途）

## 功能特點

- 特點 1
- 特點 2
- 特點 3

## 安裝需求

- Python 版本
- 必要套件
- 系統需求

## 快速開始

### 安裝
\`\`\`bash
# 安裝步驟
\`\`\`

### 使用方式
\`\`\`bash
# 基本使用範例
\`\`\`

## 詳細文檔

連結到其他文件

## 授權

授權資訊
```

**最佳實踐：**
- 使用清晰的標題層次
- 加入實際的程式碼範例
- 提供螢幕截圖或 GIF（如適用）
- 保持簡潔，詳細內容放在其他文件
- 加入徽章（Badge）顯示狀態

**範例徽章：**
```markdown
![Python Version](https://img.shields.io/badge/python-3.7+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
```

---

### 2. QUICKSTART.md（建議）

**目的：** 讓使用者在最短時間內開始使用

**應包含：**
- 30 秒快速上手指南
- 最小化的安裝步驟
- 一個完整的使用範例
- 常見問題快速解答

**範例結構：**
```markdown
# 快速開始

## 3 步驟開始使用

1. 安裝
2. 配置
3. 執行

## 第一個範例

完整的工作範例

## 常見問題

Q: 問題 1?
A: 答案 1

Q: 問題 2?
A: 答案 2
```

---

### 3. CLAUDE.md（使用 Claude Code 時）

**目的：** 為 Claude Code 提供專案背景知識

**應包含：**
```markdown
# CLAUDE.md

## 專案概述
簡要說明專案用途和核心功能

## Python 環境
- Python 版本
- 套件管理工具（uv/pip）
- 主要依賴

## 程式架構
- 主要檔案說明
- 核心邏輯位置
- 重要函數/類別

## 執行方式
實際的執行命令

## 修改時注意事項
- 關鍵的設計決策
- 需要注意的限制
- 測試方法

## 專案文件
列出所有相關文件
```

**為什麼需要？**
- Claude Code 會自動讀取此檔案
- 幫助 AI 更好地理解專案
- 提高開發效率
- 減少錯誤

---

## 配置檔案說明

### 1. pyproject.toml（必要）

**目的：** Python 專案的標準配置檔案（PEP 518/621）

**基本結構：**
```toml
[project]
name = "my-project"
version = "1.0.0"
description = "專案簡短描述"
readme = "README.md"
requires-python = ">=3.7"
license = {text = "MIT"}
authors = [
    {name = "Your Name", email = "your.email@example.com"}
]
keywords = ["keyword1", "keyword2"]
classifiers = [
    "Development Status :: 4 - Beta",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.7",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
]

# 專案依賴
dependencies = [
    "requests>=2.28.0",
    "pandas>=1.5.0",
]

# 可選依賴（例如開發工具）
[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=22.0.0",
    "ruff>=0.0.250",
]

# 專案 URL
[project.urls]
Homepage = "https://github.com/username/my-project"
Documentation = "https://my-project.readthedocs.io"
Repository = "https://github.com/username/my-project.git"
"Bug Tracker" = "https://github.com/username/my-project/issues"

# 命令行工具（如果有）
[project.scripts]
my-tool = "my_project.main:main"

# 建置系統（使用 uv 時）
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

# 工具配置（可選）
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"

[tool.black]
line-length = 88
target-version = ['py37']

[tool.ruff]
line-length = 88
select = ["E", "F", "W"]
```

**重要欄位說明：**
- `name`: 專案名稱（發布到 PyPI 時使用）
- `version`: 版本號（遵循語義化版本 SemVer）
- `dependencies`: 執行時必要的套件
- `requires-python`: 支援的 Python 版本範圍

---

### 2. .gitignore（必要）

**目的：** 告訴 Git 哪些檔案不要追蹤

**Python 專案標準範本：**
```gitignore
# Python 編譯檔案
__pycache__/
*.py[cod]
*$py.class
*.so
.Python

# 虛擬環境
venv/
env/
ENV/
.venv
.env

# uv
.uv/

# 套件分發
dist/
build/
*.egg-info/
*.egg

# 測試與覆蓋率
.pytest_cache/
.coverage
htmlcov/
.tox/

# IDE
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store

# 專案特定檔案
*.log
*.db
*.sqlite

# 環境變數檔案（包含敏感資訊）
.env
.env.local
secrets.json
config.local.json

# 作業資料夾（依專案需求）
data/raw/*
output/*
temp/*

# 保留資料夾結構（使用 .gitkeep）
!data/.gitkeep
!output/.gitkeep
!temp/.gitkeep

# OS 檔案
.DS_Store
Thumbs.db
desktop.ini

# 文件建置輸出
docs/_build/
site/
```

**分類說明：**
1. **Python 相關**：編譯檔、快取
2. **環境相關**：虛擬環境、環境變數
3. **建置相關**：dist、build 資料夾
4. **IDE 相關**：編輯器配置檔
5. **專案特定**：根據需求自訂

**如何保留空資料夾：**
```bash
# 在空資料夾中建立 .gitkeep
touch data/.gitkeep
touch output/.gitkeep
```

---

### 3. requirements.txt（傳統方式）

**目的：** 記錄專案依賴（使用 pip 時）

**格式：**
```txt
# 生產環境依賴
requests>=2.28.0,<3.0.0
pandas>=1.5.0
numpy==1.24.0

# 開發依賴（通常放在 requirements-dev.txt）
pytest>=7.0.0
black>=22.0.0
```

**版本指定方式：**
```txt
package==1.0.0      # 固定版本
package>=1.0.0      # 最小版本
package>=1.0.0,<2.0 # 版本範圍
package~=1.4.2      # 相容版本（>=1.4.2, <1.5.0）
```

**生成方式：**
```bash
# 從當前環境生成
pip freeze > requirements.txt

# 只列出直接依賴（推薦）
pip list --format=freeze > requirements.txt
```

**注意：** 使用 uv 時，這個檔案是可選的，因為 `pyproject.toml` 和 `uv.lock` 已經管理依賴。

---

### 4. uv.lock（uv 自動生成）

**目的：** 鎖定所有依賴的精確版本

**特點：**
- 自動生成，不需手動編輯
- 包含所有直接和間接依賴
- 確保在不同環境中獲得相同的依賴版本
- 類似 npm 的 `package-lock.json`

**何時更新：**
- 執行 `uv sync` 時
- 執行 `uv add package` 時
- 執行 `uv remove package` 時

**應該提交到 Git 嗎？**
✅ **是的！** 應該提交到版本控制，確保團隊成員使用相同版本。

---

### 5. .editorconfig（建議）

**目的：** 統一不同編輯器的程式碼格式

**範例：**
```ini
# EditorConfig 配置檔案
root = true

# 所有檔案
[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

# Python 檔案
[*.py]
indent_style = space
indent_size = 4
max_line_length = 88

# YAML 檔案
[*.{yml,yaml}]
indent_style = space
indent_size = 2

# Markdown 檔案
[*.md]
trim_trailing_whitespace = false

# JSON 檔案
[*.json]
indent_style = space
indent_size = 2
```

**支援的編輯器：**
- VSCode（需安裝 EditorConfig 擴充套件）
- PyCharm（內建支援）
- Sublime Text
- Vim
- 其他主流編輯器

---

## 環境設定檔案

### 1. .vscode/settings.json

**目的：** VSCode 專案特定設定

**推薦配置：**
```json
{
  // Python 解釋器
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",

  // 格式化工具
  "python.formatting.provider": "black",
  "editor.formatOnSave": true,

  // Linting
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.ruffEnabled": true,

  // 測試
  "python.testing.pytestEnabled": true,
  "python.testing.unittestEnabled": false,

  // 編輯器設定
  "editor.rulers": [88],
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,

  // 排除檔案
  "files.exclude": {
    "**/__pycache__": true,
    "**/*.pyc": true,
    ".pytest_cache": true,
    ".venv": true
  }
}
```

---

### 2. .vscode/launch.json

**目的：** 除錯配置

**範例：**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: 目前檔案",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "justMyCode": true
    },
    {
      "name": "Python: 主程式",
      "type": "python",
      "request": "launch",
      "program": "${workspaceFolder}/main.py",
      "console": "integratedTerminal",
      "args": []
    },
    {
      "name": "Python: pytest",
      "type": "python",
      "request": "launch",
      "module": "pytest",
      "args": [
        "tests",
        "-v"
      ],
      "console": "integratedTerminal"
    }
  ]
}
```

---

### 3. .vscode/extensions.json

**目的：** 推薦團隊安裝的 VSCode 擴充套件

**範例：**
```json
{
  "recommendations": [
    "ms-python.python",
    "ms-python.vscode-pylance",
    "charliermarsh.ruff",
    "ms-python.black-formatter",
    "tamasfe.even-better-toml",
    "editorconfig.editorconfig",
    "streetsidesoftware.code-spell-checker"
  ]
}
```

---

## 建立新專案的步驟

### 方法 1：使用 uv（推薦）

```bash
# 1. 建立專案資料夾
mkdir my-project
cd my-project

# 2. 初始化 uv 專案
uv init

# 3. 建立虛擬環境
uv sync

# 4. 初始化 Git
git init

# 5. 建立 .gitignore
curl -o .gitignore https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore

# 6. 建立 README.md
echo "# My Project" > README.md

# 7. 建立主程式
touch main.py

# 8. 第一次 commit
git add .
git commit -m "Initial commit"
```

---

### 方法 2：手動建立完整結構

```bash
# 建立專案結構
mkdir -p my-project/{src,tests,docs,data,examples}
cd my-project

# 建立基本檔案
touch README.md
touch QUICKSTART.md
touch CLAUDE.md
touch .gitignore
touch .editorconfig
touch pyproject.toml
touch src/__init__.py
touch src/main.py
touch tests/__init__.py

# 建立 VSCode 配置
mkdir -p .vscode
touch .vscode/settings.json
touch .vscode/launch.json
touch .vscode/extensions.json

# 初始化 Git
git init

# 初始化 uv
uv init
uv sync
```

---

## 最佳實踐建議

### 1. 文件撰寫原則

✅ **該做的：**
- 使用清晰的標題層次
- 提供實際可執行的程式碼範例
- 保持文件與程式碼同步更新
- 使用相對連結連結其他文件
- 加入目錄（Table of Contents）

❌ **不該做的：**
- 把所有內容塞在一個檔案
- 使用模糊不清的描述
- 假設讀者知道所有背景知識
- 忘記更新過時的資訊

---

### 2. 版本控制原則

**一定要提交：**
- ✅ 原始碼（.py 檔案）
- ✅ 配置檔案（pyproject.toml）
- ✅ 文件（.md 檔案）
- ✅ .gitignore
- ✅ uv.lock（依賴鎖定）
- ✅ 測試程式碼

**一定不要提交：**
- ❌ 虛擬環境（.venv/）
- ❌ 編譯檔案（__pycache__/）
- ❌ 密碼、API 金鑰（.env）
- ❌ IDE 個人設定（.vscode/settings.local.json）
- ❌ 大型資料檔案（使用 Git LFS）
- ❌ 作業產生的檔案（logs, cache）

---

### 3. 依賴管理原則

**使用 uv：**
```bash
# 新增依賴
uv add requests

# 新增開發依賴
uv add --dev pytest

# 移除依賴
uv remove requests

# 更新所有依賴
uv sync --upgrade

# 安裝專案（包含依賴）
uv sync
```

**版本指定建議：**
- 生產依賴：使用範圍版本（`>=1.0.0,<2.0.0`）
- 開發工具：可以更寬鬆（`>=1.0.0`）
- 關鍵套件：考慮固定版本（`==1.2.3`）

---

### 4. 目錄結構建議

**小型專案（單一檔案）：**
```
my-script/
├── script.py
├── README.md
├── requirements.txt
└── .gitignore
```

**中型專案（多個模組）：**
```
my-package/
├── src/
│   ├── __init__.py
│   ├── main.py
│   └── utils.py
├── tests/
│   └── test_main.py
├── README.md
├── pyproject.toml
└── .gitignore
```

**大型專案（套件）：**
```
my-app/
├── src/
│   └── my_app/
│       ├── __init__.py
│       ├── cli/
│       ├── core/
│       └── utils/
├── tests/
├── docs/
├── examples/
├── README.md
├── pyproject.toml
└── .gitignore
```

---

## 不同類型專案的結構

### 1. 命令列工具（CLI Tool）

```
cli-tool/
├── src/
│   └── my_cli/
│       ├── __init__.py
│       ├── __main__.py          # 進入點
│       ├── cli.py               # 命令列介面
│       ├── commands/            # 各種命令
│       │   ├── init.py
│       │   └── run.py
│       └── utils.py
├── tests/
├── README.md
└── pyproject.toml

# pyproject.toml 設定進入點
[project.scripts]
my-tool = "my_cli.cli:main"
```

---

### 2. Web API（FastAPI/Flask）

```
api-project/
├── src/
│   └── my_api/
│       ├── __init__.py
│       ├── main.py              # FastAPI app
│       ├── models/              # 資料模型
│       ├── routes/              # API 路由
│       ├── services/            # 業務邏輯
│       └── config.py            # 配置
├── tests/
├── alembic/                     # 資料庫遷移
├── .env.example                 # 環境變數範例
├── docker-compose.yml           # Docker 配置
├── README.md
└── pyproject.toml
```

---

### 3. 資料科學專案

```
data-science-project/
├── notebooks/                   # Jupyter notebooks
│   ├── 01_exploration.ipynb
│   └── 02_modeling.ipynb
├── src/
│   └── analysis/
│       ├── data_processing.py
│       └── visualization.py
├── data/
│   ├── raw/                     # 原始資料（不提交）
│   ├── processed/               # 處理後資料
│   └── .gitkeep
├── models/                      # 訓練好的模型
├── reports/                     # 分析報告
├── requirements.txt
└── README.md
```

---

### 4. 套件（Library）

```
my-library/
├── src/
│   └── my_lib/
│       ├── __init__.py
│       ├── core.py
│       └── utils.py
├── tests/
├── docs/
│   ├── conf.py                  # Sphinx 配置
│   └── index.rst
├── examples/
├── LICENSE
├── README.md
├── CHANGELOG.md
└── pyproject.toml
```

---

## 檢查清單

建立新專案時，確保包含以下項目：

### 必要檔案 ✅
- [ ] README.md - 專案說明
- [ ] pyproject.toml - 專案配置
- [ ] .gitignore - Git 忽略規則
- [ ] 主程式碼檔案

### 強烈建議 📝
- [ ] QUICKSTART.md - 快速開始
- [ ] CLAUDE.md - AI 助手指引（使用 Claude Code 時）
- [ ] .editorconfig - 編輯器配置
- [ ] tests/ - 測試程式碼
- [ ] LICENSE - 授權條款（開源專案）

### 依需求新增 🔧
- [ ] CONTRIBUTING.md - 貢獻指南
- [ ] CHANGELOG.md - 更新日誌
- [ ] requirements.txt - 傳統依賴清單
- [ ] .vscode/ - VSCode 配置
- [ ] docker-compose.yml - Docker 配置
- [ ] .github/ - GitHub Actions

---

## 常見問題

### Q1: pyproject.toml 和 requirements.txt 都需要嗎？

**A:** 如果使用 uv，只需要 `pyproject.toml`。但保留 `requirements.txt` 可以：
- 相容舊的工作流程
- 給不使用 uv 的使用者參考
- 用於某些 CI/CD 系統

### Q2: 該把 .vscode/ 提交到 Git 嗎？

**A:** 分情況：
- ✅ **團隊專案**：建議提交共用設定（settings.json, launch.json）
- ❌ **個人專案**：可以不提交
- 💡 **最佳做法**：提交專案特定設定，個人偏好用 `.vscode/settings.local.json`（加入 .gitignore）

### Q3: uv.lock 該提交嗎？

**A:** ✅ **是的！** 應該提交。
- 確保所有人使用相同版本
- 類似 npm 的 package-lock.json
- 重現 bug 時很重要

### Q4: 如何管理敏感資訊（API 金鑰、密碼）？

**A:**
1. 建立 `.env` 檔案儲存敏感資訊
2. 在 `.gitignore` 加入 `.env`
3. 建立 `.env.example` 作為範本（不包含實際值）
4. 在文件中說明需要設定哪些環境變數

```bash
# .env.example
API_KEY=your_api_key_here
DATABASE_URL=postgresql://user:password@localhost/db
```

### Q5: 專案越來越大，如何組織程式碼？

**A:** 使用清晰的目錄結構：
```
src/
└── my_project/
    ├── __init__.py
    ├── cli/          # 命令列介面
    ├── core/         # 核心邏輯
    ├── models/       # 資料模型
    ├── services/     # 服務層
    ├── utils/        # 工具函數
    └── config.py     # 配置
```

---

## 相關資源

- [Python Packaging 官方指南](https://packaging.python.org/)
- [PEP 621 - pyproject.toml 規範](https://peps.python.org/pep-0621/)
- [uv 官方文檔](https://docs.astral.sh/uv/)
- [GitHub .gitignore 範本](https://github.com/github/gitignore)
- [EditorConfig 官方網站](https://editorconfig.org/)
- [Semantic Versioning 語義化版本](https://semver.org/lang/zh-TW/)

---

## 總結

一個完整的專案包含：

1. **程式碼** - 實際功能
2. **文件** - 讓人理解如何使用
3. **配置** - 管理依賴和環境
4. **測試** - 確保品質
5. **版本控制** - 追蹤變更

建立專案時，從基本結構開始，隨專案成長逐步加入更多檔案。

**記住：** 好的專案結構讓協作更容易，讓未來的自己感謝現在的自己！
