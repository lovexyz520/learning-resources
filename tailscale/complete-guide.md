# Tailscale 使用教學（入門到實務）

本指南帶你從零開始使用 Tailscale，建立安全、可管理的私人網路（Tailnet）。

## 目錄

- [1. Tailscale 是什麼](#1-tailscale-是什麼)
- [2. 什麼情境適合用 Tailscale](#2-什麼情境適合用-tailscale)
- [3. 前置準備](#3-前置準備)
- [4. 安裝 Tailscale](#4-安裝-tailscale)
- [5. 加入 Tailnet](#5-加入-tailnet)
- [6. 基本連線驗證](#6-基本連線驗證)
- [7. MagicDNS 與主機名稱](#7-magicdns-與主機名稱)
- [8. Taildrop 檔案傳輸](#8-taildrop-檔案傳輸)
- [9. Tailscale SSH](#9-tailscale-ssh)
- [10. 裝置管理與標籤](#10-裝置管理與標籤)
- [11. Exit Node（出口節點）](#11-exit-node出口節點)
- [12. Subnet Router（子網路路由）](#12-subnet-router子網路路由)
- [13. ACL 與權限控管](#13-acl-與權限控管)
- [14. 安全建議](#14-安全建議)
- [15. 常見問題排查](#15-常見問題排查)
- [16. 上線檢查清單](#16-上線檢查清單)

---

## 1. Tailscale 是什麼

Tailscale 是基於 WireGuard 的零信任私有網路工具。  
它讓不同地點、不同網路下的裝置能安全互連，不用自己維護複雜 VPN 架構。

核心特點：
- 端到端加密
- 免手動開防火牆埠（多數情境）
- 裝置加入快、管理介面直觀
- 可細緻做權限控管（ACL）

---

## 2. 什麼情境適合用 Tailscale

- 遠端連回家中伺服器/NAS
- 團隊內部工具只給內網人員存取
- 多雲/多機房間安全連線
- 用手機安全連回公司測試環境

---

## 3. 前置準備

你需要：
- 一個 Tailscale 帳號（Google / Microsoft / GitHub / SSO）
- 至少 2 台裝置（方便測試互連）
- 管理權限（若要設定 Exit Node / Subnet Router）

---

## 4. 安裝 Tailscale

## 4.1 Windows / macOS

可用官方安裝程式（GUI）安裝，完成後登入帳號。

## 4.2 Linux（CLI）

常見流程（依發行版調整）：

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

查版本：

```bash
tailscale version
```

---

## 5. 加入 Tailnet

安裝後登入同一組織帳號，裝置會加入同一個 Tailnet。  
可在管理控制台看到所有節點。

Linux 常用：

```bash
sudo tailscale up
tailscale status
```

---

## 6. 基本連線驗證

取得另一台裝置的 Tailscale IP（通常 `100.x.y.z`）後測試：

```bash
ping <tailscale_ip>
```

也可用裝置名稱（啟用 MagicDNS 後更方便）。

---

## 7. MagicDNS 與主機名稱

開啟 MagicDNS 後，可以直接用節點名稱連線，不必記 IP。  
例如：

```bash
ping my-laptop
ssh user@my-server
```

建議：
- 統一命名規則（如 `team-env-role`）
- 避免重名與模糊名稱

---

## 8. Taildrop 檔案傳輸

Taildrop 可在 Tailnet 裝置間直接傳檔。

常見用法：
- GUI 直接選檔傳送
- CLI/系統整合（依平台能力）

適合情境：
- 臨時傳設定檔、日誌、診斷報告

---

## 9. Tailscale SSH

可用 Tailscale 管理 SSH 連線與授權，減少暴露公共 SSH 埠。

連線範例：

```bash
ssh user@my-server
```

建議搭配 ACL 限定誰可 SSH 到哪些節點。

---

## 10. 裝置管理與標籤

常見管理策略：
- 依用途加 tag（例如 `tag:prod`, `tag:dev`, `tag:nas`）
- 重要主機設定 `key expiry` 政策
- 離職/退役裝置即時移除

---

## 11. Exit Node（出口節點）

Exit Node 可讓某裝置把「全部網際網路流量」走另一個 Tailscale 節點出口。

適合：
- 公共 Wi-Fi 下提高安全
- 指定國家/地區出口

啟用後注意：
- 出口節點頻寬與延遲會影響使用體驗
- 需明確控管誰可使用 Exit Node

---

## 12. Subnet Router（子網路路由）

Subnet Router 讓 Tailnet 裝置可存取某個內網子網（例如 `192.168.1.0/24`）。

Linux 節點常見設定概念：

```bash
sudo tailscale up --advertise-routes=192.168.1.0/24
```

再到管理介面核准該路由。

---

## 13. ACL 與權限控管

ACL 用來限制：
- 哪些使用者/群組可連哪些裝置
- 哪些 tag 可以互通
- 哪些協定/port 可用

建議做法：
1. 先白名單最小權限
2. 再按需求擴權
3. 每次改 ACL 都做測試驗證

---

## 14. 安全建議

- 啟用 SSO 與 MFA
- 重要資源使用 tag + ACL 雙重控管
- 定期清查不活躍裝置
- 不把 Tailscale 當成唯一安全邊界，服務端仍要做認證授權

---

## 15. 常見問題排查

## 問題 1：裝置看得到但 ping 不通

檢查：
- 兩端是否都在線
- 系統防火牆是否擋 ICMP/流量
- ACL 是否阻擋

## 問題 2：有連上但速度慢

檢查：
- 是否走了 Exit Node
- 出口節點網路品質
- 裝置本地網路品質（Wi-Fi 訊號、上行頻寬）

## 問題 3：Subnet Route 看不到

檢查：
- 路由是否已在控制台核准
- `--advertise-routes` 是否正確
- Linux `ip_forward` 是否啟用（若場景需要）

---

## 16. 上線檢查清單

- [ ] 所有必要節點已加入 Tailnet
- [ ] MagicDNS 命名規範已落地
- [ ] ACL 已限制到最小可用權限
- [ ] Exit Node / Subnet Router 已驗證可用
- [ ] 已建立裝置退役與權限回收流程

