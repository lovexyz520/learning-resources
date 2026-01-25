# UV 使用指南

本指南介紹如何使用 [uv](https://docs.astral.sh/uv/) 管理 Python 環境和套件。

## 什麼是 uv？

uv 是一個**極快**的 Python 套件和專案管理器，用 Rust 編寫，可以替代 pip、pip-tools、virtualenv 等工具。

### 優點

- **速度極快** - 比 pip 快 10-100 倍
- **自動管理虛擬環境** - 不需要手動 activate
- **自動生成 lock 文件** - 確保可重現性
- **統一管理依賴** - 一個工具搞定所有事

---

## 安裝 uv

### Windows

```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### macOS/Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 驗證安裝

```bash
uv --version
```

---

## 基礎用法

### 1. 建立新專案

```bash
# 建立新專案（會自動建立 pyproject.toml）
uv init my-project
cd my-project

# 初始化環境
uv sync
```

### 2. 現有專案初始化

如果專案已有 `pyproject.toml`：

```bash
uv sync
```

這個命令會：
- 自動下載並安裝指定版本的 Python（如果需要）
- 建立虛擬環境（.venv 資料夾）
- 安裝所有依賴
- 生成 uv.lock 文件

### 3. 執行 Python 腳本

```bash
uv run python your_script.py
```

`uv run` 會自動：
- 啟用虛擬環境
- 執行指定的命令
- 不需要手動 activate 虛擬環境

### 4. 執行 Python 命令

```bash
# 執行單行程式碼
uv run python -c "print('Hello')"

# 檢查 Python 版本
uv run python --version

# 進入互動式 Python shell
uv run python
```

---

## 套件管理

### 新增依賴套件

```bash
# 新增套件
uv add requests

# 新增開發用套件
uv add --dev pytest

# 新增特定版本
uv add "requests>=2.28.0"
```

這會：
- 將套件加入 pyproject.toml
- 安裝套件到虛擬環境
- 更新 uv.lock

### 移除依賴套件

```bash
uv remove requests
```

### 查看已安裝的套件

```bash
uv pip list
```

### 更新所有依賴

```bash
uv sync --upgrade
```

### 清理快取

```bash
uv cache clean
```

---

## 專案檔案說明

### pyproject.toml

專案配置文件，定義：
- 專案名稱、版本、描述
- Python 版本需求
- 依賴套件列表

```toml
[project]
name = "my-project"
version = "1.0.0"
description = "專案描述"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.28.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
]
```

### uv.lock

自動生成的 lock 文件，記錄：
- 所有依賴的精確版本
- 確保在不同環境中獲得相同的依賴版本
- 類似 npm 的 package-lock.json

**注意：** 這個文件應該提交到 Git

### .venv/

虛擬環境資料夾（由 `uv sync` 自動建立）
- 包含獨立的 Python 解釋器和套件
- 不應該提交到 Git（記得加入 .gitignore）

---

## 與傳統 Python 工具的對比

| 功能 | 傳統方式 | uv 方式 |
|------|---------|---------|
| 建立虛擬環境 | `python -m venv .venv` | `uv sync` |
| 啟用虛擬環境 | Windows: `.venv\Scripts\activate`<br>Unix: `source .venv/bin/activate` | 不需要！使用 `uv run` |
| 安裝套件 | `pip install package` | `uv add package` |
| 執行腳本 | `python script.py` | `uv run python script.py` |
| 固定依賴版本 | `pip freeze > requirements.txt` | 自動生成 `uv.lock` |

---

## 團隊協作

### 給其他開發者

當其他人 clone 專案時，只需要：

```bash
# 1. 安裝 uv（一次性）
# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. 初始化環境
uv sync

# 3. 執行程式
uv run python main.py
```

### Git 工作流程

```bash
# clone 專案
git clone <repository-url>
cd my-project

# 初始化環境
uv sync

# 開發...
uv run python main.py

# 如果修改了 pyproject.toml，記得同步
uv sync

# 提交時包含 uv.lock
git add pyproject.toml uv.lock
git commit -m "Update dependencies"
```

---

## 進階用法

### 指定 Python 版本

```bash
# 在 pyproject.toml 中指定
requires-python = ">=3.10"

# 或使用特定版本
uv python pin 3.11
```

### 執行專案腳本

在 pyproject.toml 中定義腳本：

```toml
[project.scripts]
myapp = "mypackage.main:main"
```

然後執行：

```bash
uv run myapp
```

### 從 requirements.txt 遷移

```bash
# 從現有的 requirements.txt 安裝
uv pip install -r requirements.txt

# 或將依賴轉換到 pyproject.toml
uv add $(cat requirements.txt)
```

---

## 疑難排解

### Q: 執行 `uv sync` 時出現錯誤？

確認：
1. uv 已正確安裝：`uv --version`
2. pyproject.toml 格式正確
3. 網路連線正常

### Q: 為什麼要用 `uv run` 而不是直接 `python`？

直接使用 `python` 會使用系統的 Python，而不是專案的虛擬環境。`uv run` 確保使用正確的環境和依賴版本。

### Q: 可以混用 uv 和 pip 嗎？

不建議。選擇一種工具並統一使用，以避免依賴衝突。

### Q: 如何在 CI/CD 中使用 uv？

```yaml
# GitHub Actions 範例
- name: Install uv
  uses: astral-sh/setup-uv@v4

- name: Install dependencies
  run: uv sync

- name: Run tests
  run: uv run pytest
```

---

## 常用命令速查表

| 命令 | 說明 |
|------|------|
| `uv init` | 建立新專案 |
| `uv sync` | 同步環境和依賴 |
| `uv run <command>` | 在虛擬環境中執行命令 |
| `uv add <package>` | 新增套件 |
| `uv add --dev <package>` | 新增開發用套件 |
| `uv remove <package>` | 移除套件 |
| `uv pip list` | 列出已安裝套件 |
| `uv sync --upgrade` | 更新所有套件 |
| `uv cache clean` | 清理快取 |
| `uv python list` | 列出可用 Python 版本 |
| `uv python pin <version>` | 指定 Python 版本 |

---

## 更多資源

- [uv 官方文檔](https://docs.astral.sh/uv/)
- [uv GitHub](https://github.com/astral-sh/uv)
- [pyproject.toml 規範](https://packaging.python.org/en/latest/specifications/pyproject-toml/)
