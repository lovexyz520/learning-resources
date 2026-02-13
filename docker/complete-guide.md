# Docker 使用教學（完整入門到進階實戰）

這份教學整理 Docker 從入門到進階的實作流程，包含安裝、核心指令、`Dockerfile`、`Compose`、安全與 CI/CD，最後銜接到 Kubernetes。

## 目錄

- [1. Docker 是什麼](#1-docker-是什麼)
- [2. 安裝與確認](#2-安裝與確認)
- [3. 核心觀念](#3-核心觀念)
- [4. 常用指令速查](#4-常用指令速查)
- [5. 第一個實作：用 Nginx 快速架站](#5-第一個實作用-nginx-快速架站)
- [6. 建立自己的映像檔（Dockerfile）](#6-建立自己的映像檔dockerfile)
- [7. 資料持久化（Volume）](#7-資料持久化volume)
- [8. 容器網路（Network）](#8-容器網路network)
- [9. 使用 Docker Compose 管理多服務](#9-使用-docker-compose-管理多服務)
- [10. 除錯與排錯](#10-除錯與排錯)
- [11. Dockerfile 最佳化（進階）](#11-dockerfile-最佳化進階)
- [12. 映像檔安全與維運（進階）](#12-映像檔安全與維運進階)
- [13. 私有 Registry 與版本策略（進階）](#13-私有-registry-與版本策略進階)
- [14. CI/CD 自動化部署（進階）](#14-cicd-自動化部署進階)
- [15. 觀測性：日誌、指標、追蹤（進階）](#15-觀測性日誌指標追蹤進階)
- [16. Docker Compose 進階模式](#16-docker-compose-進階模式)
- [17. 從 Docker 走向 Kubernetes（銜接路線）](#17-從-docker-走向-kubernetes銜接路線)
- [18. 常見問題（FAQ）](#18-常見問題faq)
- [19. 實戰練習任務](#19-實戰練習任務)
- [20. 總結](#20-總結)

---

## 1. Docker 是什麼

Docker 是一種容器化平台，能把應用程式與執行環境一起封裝為 `image`，再以 `container` 方式執行。你可以把它理解成「可重現、可攜帶、可快速啟動」的執行單位。

主要優點：
- 環境一致：開發、測試、正式環境差異大幅降低。
- 啟動快速：容器通常比傳統虛擬機更快啟動。
- 便於部署：可搭配 `registry`、`CI/CD`、`Docker Compose` 進行自動化。

## 2. 安裝與確認

### Windows（建議）
1. 安裝 `Docker Desktop`。
2. 啟用 `WSL 2`（若系統提示）。
3. 重新開機後啟動 Docker Desktop。

### macOS
1. 安裝 `Docker Desktop`。
2. 啟動應用程式並完成初始化。

### Linux
1. 安裝 `Docker Engine`。
2. 啟用並啟動服務：

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

### 驗證安裝

```bash
docker --version
docker run hello-world
```

若看到 `Hello from Docker!` 代表安裝成功。

## 3. 核心觀念

- `image`：唯讀模板，包含程式與依賴。
- `container`：`image` 的執行實例。
- `Dockerfile`：建立 `image` 的腳本。
- `registry`：儲存與分發 `image`（例如 `Docker Hub`）。
- `volume`：資料持久化儲存，避免容器刪除後資料遺失。
- `network`：容器間或容器對外連線的虛擬網路。

## 4. 常用指令速查

```bash
# 拉取映像檔
docker pull nginx:latest

# 查看本機映像檔
docker images

# 啟動容器（背景執行）
docker run -d --name web -p 8080:80 nginx:latest

# 查看執行中的容器
docker ps

# 查看所有容器（含停止）
docker ps -a

# 查看日誌
docker logs web

# 進入容器 shell
docker exec -it web sh

# 停止 / 刪除容器
docker stop web
docker rm web

# 刪除映像檔
docker rmi nginx:latest
```

## 5. 第一個實作：用 Nginx 快速架站

1. 啟動容器：

```bash
docker run -d --name my-nginx -p 8080:80 nginx:latest
```

2. 開啟瀏覽器進入 `http://localhost:8080`。
3. 停止服務：

```bash
docker stop my-nginx
```

4. 移除容器：

```bash
docker rm my-nginx
```

## 6. 建立自己的映像檔（Dockerfile）

### 範例：Node.js 應用

建立 `Dockerfile`：

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

建置與執行：

```bash
docker build -t my-node-app:1.0 .
docker run -d --name node-app -p 3000:3000 my-node-app:1.0
```

## 7. 資料持久化（Volume）

若資料只放在容器內，刪除容器後會遺失。使用 `volume` 可保留資料。

```bash
# 建立 volume
docker volume create mysql-data

# 掛載 volume 執行 MySQL
docker run -d --name mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -v mysql-data:/var/lib/mysql \
  -p 3306:3306 mysql:8
```

查看 volume：

```bash
docker volume ls
```

## 8. 容器網路（Network）

讓多個容器在同一虛擬網路互相用名稱連線。

```bash
# 建立網路
docker network create app-net

# 啟動資料庫容器
docker run -d --name db --network app-net -e MYSQL_ROOT_PASSWORD=secret mysql:8

# 啟動後端容器（可用 db 作為主機名稱）
docker run -d --name api --network app-net my-api:1.0
```

## 9. 使用 Docker Compose 管理多服務

當服務超過一個（例如前端、後端、資料庫），建議使用 `Docker Compose`。

建立 `compose.yaml`：

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  api:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

啟動與關閉：

```bash
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

## 10. 除錯與排錯

常見檢查流程：
1. 看容器狀態：`docker ps -a`
2. 看日誌：`docker logs <container>`
3. 進入容器：`docker exec -it <container> sh`
4. 檢查連接埠衝突：是否已被其他程式占用。
5. 檢查環境變數：是否設定錯誤（例如資料庫密碼）。
6. 檢查網路：容器是否在同一 `network`。

## 11. Dockerfile 最佳化（進階）

### 11.1 多階段建置（Multi-stage build）

用多階段可降低最終 `image` 大小，並避免把建置工具帶進正式環境。

```dockerfile
# build stage
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# runtime stage
FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

### 11.2 `.dockerignore`

建立 `.dockerignore` 以避免把無用檔案送進建置上下文。

```text
node_modules
.git
.env
coverage
dist
*.log
```

### 11.3 快取優化

- 先複製 `package*.json` 再安裝依賴，提升 `layer` 快取命中率。
- 不常變動指令放前面，常變動內容放後面。

## 12. 映像檔安全與維運（進階）

### 12.1 基礎安全原則

- 使用可信來源基底映像檔。
- 固定版本標籤，不只用 `latest`。
- 容器盡量以非 `root` 身分執行。

### 12.2 漏洞掃描

可用 `Trivy` 或 `Docker Scout` 檢查弱點。

```bash
# Trivy 範例
trivy image my-node-app:1.0
```

### 12.3 資源限制

避免單一容器耗盡主機資源。

```bash
docker run -d --name app \
  --cpus="1.0" \
  --memory="512m" \
  my-node-app:1.0
```

## 13. 私有 Registry 與版本策略（進階）

### 13.1 登入與推送

```bash
docker login
docker tag my-node-app:1.0 myrepo/my-node-app:1.0
docker push myrepo/my-node-app:1.0
```

### 13.2 版本標籤建議

- `1.2.3`：正式釋出版本。
- `1.2`：次版本線。
- `prod`、`staging`：環境標籤（需搭配流程管理）。
- `latest`：僅做開發測試輔助，不建議作為正式部署依據。

## 14. CI/CD 自動化部署（進階）

以下示範 `GitHub Actions` 自動建置與推送映像檔：

```yaml
name: docker-build-push
on:
  push:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set up Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build and Push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: myrepo/my-node-app:latest,myrepo/my-node-app:${{ github.sha }}
```

建議流程：
1. 推送程式碼到 `main`。
2. `CI` 自動建置、測試、掃描弱點。
3. 通過後推送到 `registry`。
4. 部署系統拉取指定版本並滾動更新。

## 15. 觀測性：日誌、指標、追蹤（進階）

### 15.1 日誌

- 先用 `docker logs` 快速查。
- 正式環境建議集中式日誌（例如 `ELK` 或 `Loki`）。

### 15.2 指標監控

可搭配 `Prometheus` + `Grafana` 觀察：
- `CPU`、記憶體、網路流量
- 容器重啟次數
- 應用程式回應時間與錯誤率

### 15.3 健康檢查（Healthcheck）

在 `Dockerfile` 或 `compose.yaml` 設定健康檢查，讓系統可偵測異常容器。

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s \
  CMD wget -qO- http://localhost:3000/health || exit 1
```

## 16. Docker Compose 進階模式

### 16.1 開發與正式環境分檔

- `compose.yaml`：共用設定
- `compose.dev.yaml`：開發覆寫
- `compose.prod.yaml`：正式覆寫

```bash
# 開發
docker compose -f compose.yaml -f compose.dev.yaml up -d

# 正式
docker compose -f compose.yaml -f compose.prod.yaml up -d
```

### 16.2 服務啟動依賴

`depends_on` 只保證啟動順序，不保證服務「可用」。需搭配健康檢查與重試機制。

## 17. 從 Docker 走向 Kubernetes（銜接路線）

### 17.1 何時需要 Kubernetes

- 服務數量增長，需自動擴縮與高可用。
- 多節點調度、滾動更新、服務發現需求提升。

### 17.2 概念對照

- Docker `container` 對應到 Kubernetes `Pod` 內的容器。
- `Docker Compose` 的多服務協調，對應到 Kubernetes 的 `Deployment`、`Service`、`ConfigMap`、`Secret`。

### 17.3 最小學習路線

1. 用 `Docker` 熟悉映像檔與容器。
2. 用 `Docker Compose` 熟悉多服務協作。
3. 學習 Kubernetes 基本物件：`Pod`、`Deployment`、`Service`。
4. 實作一次從 `CI` 到 Kubernetes 的部署流程。

## 18. 常見問題（FAQ）

### Q1：容器一啟動就退出怎麼辦？

A：先看 `docker logs <container>`，通常是啟動命令錯誤、環境變數缺失或應用程式崩潰。

### Q2：為什麼改了程式碼但容器沒更新？

A：可能是快取命中舊 `layer`，請重新 `build` 並確認標籤是否正確。

### Q3：資料庫資料為什麼消失？

A：未使用 `volume` 持久化，刪除容器後資料會遺失。

### Q4：正式環境可以直接用 `latest` 嗎？

A：不建議。正式環境應指定固定版本標籤，確保可追蹤與可回滾。

## 19. 實戰練習任務

1. 練習 1：啟動 `nginx` 並透過瀏覽器存取。
2. 練習 2：建立自己的 `Dockerfile` 並成功執行。
3. 練習 3：讓 `api` 容器連到 `mysql` 容器。
4. 練習 4：用 `Docker Compose` 一次啟動三個服務。
5. 練習 5：用 `GitHub Actions` 自動推送映像檔到 `registry`。
6. 練習 6：替容器加入健康檢查與資源限制。
7. 練習 7：把同一個應用拆成開發與正式兩組 Compose 設定。

## 20. 總結

你現在有一份可直接落地的 Docker 教學，涵蓋：
- 入門操作與核心觀念
- `Dockerfile`、`volume`、`network`、`Docker Compose`
- 安全、版本、`CI/CD`、監控
- 銜接 `Kubernetes` 的實務路線

依照本教學逐步實作，你可以從單機開發穩定走到團隊級部署流程。
