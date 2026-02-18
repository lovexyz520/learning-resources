# PowerShell 完整操作與檔案管理實戰（學習與教學版）

本文件是可直接拿來當「學習筆記 + 教學講義」的完整版本，包含：
- PowerShell 基本命令與觀念
- 常用系統與檔案操作
- 腳本化與錯誤處理
- 檔案管理實戰（壓縮、批次改名、搜尋清理）
- 排程自動化與安全實務

## 目錄

- [1. 先理解 PowerShell 的核心](#1-先理解-powershell-的核心)
- [2. 基本命令與別名對照](#2-基本命令與別名對照)
- [3. 導航與目錄操作（`pwd` `ls` `cd` `clear`）](#3-導航與目錄操作-pwd-ls-cd-clear)
- [4. 檔案與資料夾操作](#4-檔案與資料夾操作)
- [5. 查詢、過濾、排序、搜尋](#5-查詢過濾排序搜尋)
- [6. 變數、條件、迴圈、函式](#6-變數條件迴圈函式)
- [7. 腳本化與參數設計](#7-腳本化與參數設計)
- [8. 錯誤處理與除錯](#8-錯誤處理與除錯)
- [9. JSON、CSV、API 實務](#9-jsoncsvapi-實務)
- [10. PowerShell 檔案管理實戰](#10-powershell-檔案管理實戰)
- [11. 排程自動化](#11-排程自動化)
- [12. 安全與最佳實踐](#12-安全與最佳實踐)
- [13. 教學用練習題](#13-教學用練習題)
- [14. 常見問題（FAQ）](#14-常見問題faq)

---

## 1. 先理解 PowerShell 的核心

PowerShell 和傳統命令列最大差異：
- 傳統 shell 多半傳字串
- PowerShell 管線傳「物件」

```powershell
Get-Process | Select-Object -First 5 Name, Id, CPU
```

這行不是在切字串，而是把 `Process` 物件傳給下一個命令處理。

常用管線命令：
- `Where-Object`：篩選
- `Select-Object`：選欄位或前幾筆
- `Sort-Object`：排序
- `Group-Object`：分組
- `Measure-Object`：統計

---

## 2. 基本命令與別名對照

PowerShell 允許你用 Linux 風格的短命令（別名），但正式腳本建議用完整命令。

| 常用寫法 | 真實命令 | 功能 |
|---|---|---|
| `ls` | `Get-ChildItem` | 列目錄內容 |
| `cd` | `Set-Location` | 切換路徑 |
| `pwd` | `Get-Location` | 顯示目前路徑 |
| `clear` | `Clear-Host` | 清畫面 |
| `cat` | `Get-Content` | 讀檔 |
| `cp` | `Copy-Item` | 複製 |
| `mv` | `Move-Item` | 移動 |
| `ren` | `Rename-Item` | 改名 |
| `mkdir` | `New-Item -ItemType Directory` | 建資料夾 |
| `ni` | `New-Item` | 建新項目 |

查別名：

```powershell
Get-Alias ls
Get-Alias cd
```

---

## 3. 導航與目錄操作（`pwd` `ls` `cd` `clear`）

### 3.1 `pwd`：你目前在哪裡

```powershell
pwd
Get-Location
```

### 3.2 `ls`：這裡有什麼

```powershell
ls
ls -Force
ls -Recurse
ls *.md
```

- `-Force`：包含隱藏項目
- `-Recurse`：遞迴列出子目錄

### 3.3 `cd`：切換位置

```powershell
cd ..
cd .\powershell
cd D:\programming-projects\learning-resources
```

### 3.4 `clear`：清空畫面

```powershell
clear
Clear-Host
```

---

## 4. 檔案與資料夾操作

### 4.1 建立

```powershell
mkdir .\demo
ni .\demo\note.txt -ItemType File
```

### 4.2 複製、移動、改名

```powershell
cp .\demo\note.txt .\demo\note-copy.txt
mv .\demo\note-copy.txt .\demo\archive.txt
ren .\demo\archive.txt note-v2.txt
```

### 4.3 讀寫檔案

```powershell
cat .\demo\note-v2.txt
"hello powershell" | Out-File .\demo\note-v2.txt -Encoding utf8
Add-Content .\demo\note-v2.txt "second line"
```

### 4.4 刪除（高風險，先預覽）

```powershell
Remove-Item .\demo\note-v2.txt -WhatIf
# 確認後再移除
# Remove-Item .\demo\note-v2.txt
```

---

## 5. 查詢、過濾、排序、搜尋

### 5.1 命令探索

```powershell
Get-Command
Get-Command *process*
Get-Help Get-Process -Examples
```

### 5.2 物件篩選與排序

```powershell
Get-Process |
    Where-Object CPU -gt 100 |
    Sort-Object CPU -Descending |
    Select-Object -First 10 Name, Id, CPU
```

### 5.3 文字搜尋（類似 grep）

```powershell
ls -Recurse -File | Select-String -Pattern "PowerShell"
```

### 5.4 檔案條件搜尋

```powershell
ls .\logs -Recurse -File -Filter *.log |
    Where-Object LastWriteTime -lt (Get-Date).AddDays(-7)
```

---

## 6. 變數、條件、迴圈、函式

### 6.1 變數與集合

```powershell
$name = "PowerShell"
$nums = 1,2,3,4,5
$config = @{
    Env = "dev"
    Retry = 3
}
```

### 6.2 條件與迴圈

```powershell
if ($config.Retry -gt 0) {
    "retry enabled"
}

foreach ($n in $nums) {
    "value: $n"
}

for ($i = 1; $i -le 3; $i++) {
    "run $i"
}
```

### 6.3 函式

```powershell
function Get-TopCpuProcess {
    param([int]$Top = 5)

    Get-Process |
        Sort-Object CPU -Descending |
        Select-Object -First $Top Name, Id, CPU
}
```

---

## 7. 腳本化與參數設計

建立 `backup.ps1`：

```powershell
param(
    [Parameter(Mandatory = $true)]
    [ValidateNotNullOrEmpty()]
    [string]$Source,

    [Parameter(Mandatory = $true)]
    [ValidateNotNullOrEmpty()]
    [string]$Destination
)

if (-not (Test-Path $Source)) {
    throw "Source not found: $Source"
}

New-Item -ItemType Directory -Path $Destination -Force | Out-Null
Copy-Item -Path $Source -Destination $Destination -Recurse -Force
Write-Host "Backup completed: $Source -> $Destination"
```

執行：

```powershell
.\backup.ps1 -Source .\data -Destination .\backup
```

---

## 8. 錯誤處理與除錯

```powershell
try {
    Get-Content .\not-exists.txt -ErrorAction Stop
}
catch {
    Write-Error "Read failed: $($_.Exception.Message)"
}
finally {
    Write-Host "Done"
}
```

除錯建議：
1. 對關鍵步驟輸出狀態（`Write-Host` / `Write-Verbose`）
2. 可能失敗的命令加 `-ErrorAction Stop`
3. VSCode 使用中斷點逐步檢查變數

---

## 9. JSON、CSV、API 實務

### 9.1 JSON

```powershell
$obj = @{
    app = "demo"
    env = "dev"
    retry = 3
}

$json = $obj | ConvertTo-Json
$json | Out-File .\config.json -Encoding utf8

$loaded = Get-Content .\config.json -Raw | ConvertFrom-Json
$loaded.env
```

### 9.2 CSV

```powershell
Get-Process |
    Select-Object Name, Id, CPU |
    Export-Csv .\process.csv -NoTypeInformation -Encoding utf8

$rows = Import-Csv .\process.csv
$rows | Select-Object -First 5
```

### 9.3 API

```powershell
$resp = Invoke-RestMethod -Method Get -Uri "https://api.github.com/repos/PowerShell/PowerShell"
$resp.full_name
```

---

## 10. PowerShell 檔案管理實戰

這段是你要的重點章節：壓縮、批次改名、搜尋清理。

### 10.1 壓縮與解壓縮

```powershell
# 建立封存目錄
mkdir .\archive -ErrorAction SilentlyContinue | Out-Null

# 壓縮 logs
Compress-Archive -Path .\logs\* -DestinationPath .\archive\logs-$(Get-Date -Format yyyyMMdd).zip -Force

# 解壓縮
Expand-Archive -Path .\archive\logs-20260218.zip -DestinationPath .\restore -Force
```

### 10.2 批次改名

```powershell
# 範例 A：副檔名轉換 .txt -> .bak
ls .\demo -File -Filter *.txt | ForEach-Object {
    Rename-Item $_.FullName ($_.BaseName + ".bak")
}

# 範例 B：加日期前綴 yyyyMMdd_
ls .\reports -File | ForEach-Object {
    $newName = "{0:yyyyMMdd}_{1}" -f (Get-Date), $_.Name
    Rename-Item $_.FullName $newName
}

# 範例 C：空白轉底線
ls .\reports -File | ForEach-Object {
    $newName = $_.Name -replace " ", "_"
    Rename-Item $_.FullName $newName
}
```

### 10.3 搜尋清理（安全版流程）

步驟一：先列出要清理的檔案

```powershell
$targets = ls .\logs -Recurse -File -Filter *.log |
    Where-Object LastWriteTime -lt (Get-Date).AddDays(-30)

$targets | Select-Object FullName, LastWriteTime
```

步驟二：先預覽刪除動作（必要）

```powershell
$targets | Remove-Item -WhatIf
```

步驟三：確認後再執行

```powershell
# $targets | Remove-Item
```

步驟四：留存清理紀錄（建議）

```powershell
$targets |
    Select-Object FullName, LastWriteTime |
    Export-Csv .\cleanup-report.csv -NoTypeInformation -Encoding utf8
```

---

## 11. 排程自動化

建立每天 08:30 執行清理腳本：

```powershell
$action = New-ScheduledTaskAction -Execute "pwsh.exe" -Argument "-File D:\scripts\daily-cleanup.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At 08:30
Register-ScheduledTask -TaskName "DailyCleanup" -Action $action -Trigger $trigger -Description "Daily log cleanup task"
```

檢查與手動啟動：

```powershell
Get-ScheduledTask -TaskName "DailyCleanup"
Start-ScheduledTask -TaskName "DailyCleanup"
```

---

## 12. 安全與最佳實踐

```powershell
Get-ExecutionPolicy -List
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

建議規則：
1. 任何刪除前都先 `-WhatIf`
2. 先備份再改名或清理
3. 機密資訊不要硬編碼在腳本
4. 重要腳本加上輸出日誌
5. 能用最小權限就不要用管理員權限

---

## 13. 教學用練習題

1. 用 `pwd`、`ls`、`cd` 完成目錄導覽並截圖記錄路徑變化。  
2. 寫 `archive-logs.ps1`：把 `logs` 目錄壓縮成日期命名 zip。  
3. 寫 `batch-rename.ps1`：把檔名空白轉底線，並加日期前綴。  
4. 寫 `cleanup-log.ps1`：先 `-WhatIf`，再輸出 `cleanup-report.csv`。  
5. 寫 `system-report.ps1`：匯出程序與服務狀態到 CSV。  

---

## 14. 常見問題（FAQ）

### Q1：為什麼 `ls`、`cd` 可以用？

A：它們是 PowerShell 內建別名，對應完整命令如 `Get-ChildItem`、`Set-Location`。

### Q2：為什麼腳本不能執行？

A：先檢查執行政策與路徑：

```powershell
Get-ExecutionPolicy -List
```

### Q3：批次改名或刪除怕出錯怎麼辦？

A：先做三件事：
1. 先列出目標檔案
2. 使用 `-WhatIf`
3. 匯出報表留紀錄
