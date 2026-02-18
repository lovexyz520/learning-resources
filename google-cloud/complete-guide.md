# Google Cloud Run 部署 Python Streamlit 教學指引

本指南帶你從零開始，把 Python Streamlit 專案部署到 Google Cloud Run。

## 目錄

- [1. 架構與目標](#1-架構與目標)
- [2. 前置準備](#2-前置準備)
- [3. 建立 Streamlit 專案](#3-建立-streamlit-專案)
- [4. 建立 Dockerfile](#4-建立-dockerfile)
- [5. 本機容器測試](#5-本機容器測試)
- [6. 設定 Google Cloud 專案](#6-設定-google-cloud-專案)
- [7. 建立 Artifact Registry](#7-建立-artifact-registry)
- [8. 建置與推送映像檔](#8-建置與推送映像檔)
- [9. 部署到 Cloud Run](#9-部署到-cloud-run)
- [10. 更新服務與回滾](#10-更新服務與回滾)
- [11. 環境變數與 Secrets](#11-環境變數與-secrets)
- [12. 權限與安全建議](#12-權限與安全建議)
- [13. 常見問題排查](#13-常見問題排查)
- [14. 上線前檢查清單](#14-上線前檢查清單)

---

## 1. 架構與目標

目標流程：
1. 本機完成 Streamlit App。
2. 用 Docker 打包成映像檔。
3. 推送到 Artifact Registry。
4. 由 Cloud Run 拉取映像並提供 HTTPS 服務。

---

## 2. 前置準備

你需要：
- Google Cloud 帳號與一個專案
- 已安裝 `gcloud` CLI
- 已安裝 Docker
- Python 3.10+（建議）

確認工具：

```bash
gcloud --version
docker --version
python --version
```

---

## 3. 建立 Streamlit 專案

建立專案檔案：

```text
my-streamlit-app/
├── app.py
├── requirements.txt
└── Dockerfile
```

`app.py`：

```python
import streamlit as st

st.set_page_config(page_title="Cloud Run Demo", page_icon=":rocket:")
st.title("Streamlit on Google Cloud Run")
st.write("Deploy successful.")
```

`requirements.txt`：

```txt
streamlit==1.39.0
```

---

## 4. 建立 Dockerfile

建立 `Dockerfile`：

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PORT=8080
EXPOSE 8080

CMD ["streamlit", "run", "app.py", "--server.port=8080", "--server.address=0.0.0.0"]
```

重點：
- Cloud Run 會用容器內指定的 port（通常 8080）
- Streamlit 必須綁定 `0.0.0.0`

---

## 5. 本機容器測試

建置映像：

```bash
docker build -t my-streamlit-app:local .
```

本機啟動：

```bash
docker run --rm -p 8080:8080 my-streamlit-app:local
```

開啟：
- `http://localhost:8080`

---

## 6. 設定 Google Cloud 專案

登入與設定專案：

```bash
gcloud auth login
gcloud config set project YOUR_PROJECT_ID
```

啟用必要 API：

```bash
gcloud services enable run.googleapis.com artifactregistry.googleapis.com cloudbuild.googleapis.com
```

---

## 7. 建立 Artifact Registry

建立 Docker repository（區域以 `asia-east1` 為例）：

```bash
gcloud artifacts repositories create streamlit-repo \
  --repository-format=docker \
  --location=asia-east1 \
  --description="Docker repo for Streamlit app"
```

---

## 8. 建置與推送映像檔

設定變數：

```bash
PROJECT_ID=YOUR_PROJECT_ID
REGION=asia-east1
REPO=streamlit-repo
IMAGE=streamlit-app
TAG=v1
IMAGE_URI=${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPO}/${IMAGE}:${TAG}
```

建置與推送（Cloud Build）：

```bash
gcloud builds submit --tag ${IMAGE_URI}
```

---

## 9. 部署到 Cloud Run

```bash
gcloud run deploy streamlit-service \
  --image ${IMAGE_URI} \
  --platform managed \
  --region ${REGION} \
  --allow-unauthenticated \
  --port 8080 \
  --cpu 1 \
  --memory 512Mi
```

部署完成後會輸出 service URL，直接打開驗證。

---

## 10. 更新服務與回滾

發新版本：
1. 更新程式碼
2. 打新 tag（例如 `v2`）
3. 重新 `gcloud builds submit --tag ...`
4. 再次 `gcloud run deploy ...`

查看修訂版本：

```bash
gcloud run revisions list --service streamlit-service --region ${REGION}
```

---

## 11. 環境變數與 Secrets

設定環境變數：

```bash
gcloud run services update streamlit-service \
  --region ${REGION} \
  --update-env-vars APP_ENV=prod,LOG_LEVEL=info
```

若有敏感資訊（API 金鑰），建議用 Secret Manager，不要寫死在程式碼或 Dockerfile。

---

## 12. 權限與安全建議

- 先用最小權限原則
- 非必要不要給專案 Owner
- 生產環境可考慮關閉 `--allow-unauthenticated` 並加 IAM 控制
- 映像檔盡量固定版本 tag，不要只用 `latest`

---

## 13. 常見問題排查

### 問題 1：服務啟動失敗

檢查 log：

```bash
gcloud run services logs read streamlit-service --region ${REGION}
```

常見原因：
- Streamlit 沒綁定 `0.0.0.0`
- 埠號不是 8080
- `requirements.txt` 缺套件

### 問題 2：部署成功但頁面打不開

- 確認是否有 `--allow-unauthenticated`
- 檢查 Cloud Run URL 是否正確
- 檢查修訂版本是否 Ready

### 問題 3：容器 build 過慢

- 減少不必要檔案（可加 `.dockerignore`）
- 盡量固定依賴版本並利用 Docker layer cache

---

## 14. 上線前檢查清單

- [ ] 本機 docker run 可正常開啟
- [ ] Cloud Build 可成功推送映像
- [ ] Cloud Run URL 可存取
- [ ] 重要設定用環境變數/Secret 管理
- [ ] 已設定資源限制（CPU/Memory）
- [ ] 已保留可回滾的舊版映像 tag

