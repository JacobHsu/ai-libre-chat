# 本機 Docker 安裝教學

本文件記錄「本專案（`d:\4-chat\LibreChat`）」在本機以 Docker 啟動時的實際設定與調整原因，方便日後重建環境或排查問題時參考。官方完整文件請見外部網站 [docs.librechat.ai](https://docs.librechat.ai)，這裡只整理本機的部分。

左側面板顯示項目的客製化設定另見 [librechat-yaml.md](./librechat-yaml.md)。

## 目前環境

| 服務 | 網址 |
|---|---|
| LibreChat | <http://localhost:3088> |
| Admin Panel | <http://localhost:3089> |

---

## 1. 為什麼要改 Port（避免與其他 LibreChat 安裝衝突）

本機同時存在另一套官方安裝（不同專案目錄），預設都使用相同的 port（`3080`、`3000`）與容器名稱，直接啟動會撞名/撞 port 而失敗。

### `.env` 調整

```dotenv
PORT=3088
DOMAIN_CLIENT=http://localhost:3088
DOMAIN_SERVER=http://localhost:3088
ADMIN_PANEL_PORT=3089
```

- `PORT`：LibreChat API / 前端服務對外的 port，`docker-compose.yml` 用 `"${PORT}:${PORT}"` 同時當 host port 與 container port。
- `DOMAIN_CLIENT` / `DOMAIN_SERVER`：必須跟 `PORT` 保持一致，否則前端 callback／CORS 等設定會錯。
- `ADMIN_PANEL_PORT`：預設不存在於 `.env`（會 fallback 到衝突的 `3000`），需自行加上。

### `docker-compose.override.yml`（容器名稱衝突）

`docker-compose.yml` 開頭寫明「不要直接修改此檔案，改用 `docker-compose.override.yaml`」。因為兩套安裝的 `container_name` 完全相同（`LibreChat`、`chat-mongodb`、`chat-meilisearch`、`vectordb`、`rag_api`、`admin-panel`），即使 port 不衝突，Docker 也會因為容器名稱重複而啟動失敗，所以在專案根目錄新增 `docker-compose.override.yml` 幫每個服務改名：

```yaml
services:
  api:
    container_name: LibreChat-4chat
  admin-panel:
    container_name: admin-panel-4chat
  mongodb:
    container_name: chat-mongodb-4chat
  meilisearch:
    container_name: chat-meilisearch-4chat
  vectordb:
    container_name: vectordb-4chat
  rag_api:
    container_name: rag_api-4chat
```

`mongodb`、`meilisearch`、`vectordb`、`rag_api` 這幾個服務本身沒有對外開 port，只有 `api` 和 `admin-panel` 需要調整 port。

### 驗證沒有衝突

```bash
docker compose config
```

確認輸出的 `container_name` 與 `ports` 都是預期值（無重複）後，再執行：

```bash
docker compose up -d
```

實測結果：兩套安裝各自使用獨立的 Docker network（`librechat_default` vs. 另一套的 `<project>_default`）與獨立的 bind-mount 資料目錄（`./data-node` 各自相對於各自的專案路徑），MongoDB 資料**完全不會互相干擾**。

---

## 2. Admin Panel 啟動失敗（`ADMIN_PANEL_SESSION_SECRET`）

`admin-panel` 容器需要至少 32 字元的 session 密鑰，沒設定的話會直接拒絕啟動並不斷重啟（Docker 面板會顯示紅燈 / `Restarting`）：

```
[admin-panel] SESSION_SECRET must be set to at least 32 characters (got 0). Refusing to start.
```

解法：在 `.env` 補上：

```dotenv
ADMIN_PANEL_SESSION_SECRET=<openssl rand -hex 32 產生的字串>
```

產生指令：

```bash
openssl rand -hex 32
```

補上後執行 `docker compose up -d admin-panel` 讓容器重建，狀態應變成 `Up ... (healthy)`。

---

## 3. 帳號登入

LibreChat 本身不內建任何預設帳號，`.env` 目前設定：

```dotenv
ALLOW_EMAIL_LOGIN=true
ALLOW_REGISTRATION=true
```

有兩種方式建立第一個帳號：

1. **網頁註冊**：開啟 <http://localhost:3088>，點選「Sign up / 註冊」自行建立。
2. **CLI 直接建立**（略過網頁註冊流程，建立後 `emailVerified=true` 可直接登入）：
   ```bash
   docker exec -it LibreChat-4chat npm run create-user -- <email> <顯示名稱> <username>
   ```
   不帶參數則會用互動方式逐步詢問。

> Admin Panel（<http://localhost:3089>）不是獨立帳號系統，需要先有一個 LibreChat 帳號並具備 `ADMIN` 角色才能登入使用。

---

## 快速指令總結

```bash
# 啟動全部服務
docker compose up -d

# 檢查狀態（確認皆為 Up / healthy）
docker compose ps

# 驗證設定檔（port、容器名稱是否有衝突）
docker compose config

# 建立第一個帳號（CLI，略過網頁註冊）
docker exec -it LibreChat-4chat npm run create-user -- <email> <name> <username>
```
