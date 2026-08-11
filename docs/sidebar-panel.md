# `librechat.yaml` 左側面板客製化教學

## 概觀

本文件說明如何透過 `librechat.yaml` 的 `interface` 區塊，調整 LibreChat 左側面板顯示的項目。這跟模型端點設定是**不同的設定區塊**（僅 Agents 端點例外，見 [model-endpoints.md 的已知限制](./model-endpoints.md#已知限制agents-端點與側欄設定衝突)）：

- Docker 安裝與 port／容器名稱衝突處理：[docker-setup.md](./docker-setup.md)
- 模型端點（Azure OpenAI 等）設定：[model-endpoints.md](./model-endpoints.md)

官方文件：[librechat.ai/docs/configuration/librechat_yaml/interface](https://www.librechat.ai/docs/configuration/librechat_yaml)

## 需求

左側面板只保留「新對話」「對話紀錄」「Skills」「附加檔案」四項，其餘全部隱藏。

## 背景

- **新對話**、**對話紀錄**、**附加檔案（Files 面板）**：這三項在前端程式碼中是無條件顯示的（無對應設定旗標），不需要、也無法透過設定關閉。
- 其餘側欄圖示（Prompts、Bookmarks、Memories、Agent/Assistant 建立器、Marketplace、MCP 伺服器管理、Parameters…）都是透過 `librechat.yaml` 的 `interface.*` 設定，同步寫入 MongoDB 的角色權限（`USER` / `ADMIN` role permissions）來控制顯示與否。
- 預設情況下，容器裡沒有掛載 `librechat.yaml`（`docker-compose.yml` 預設不掛這個檔案），啟動時會看到：
  ```
  Config file YAML format is invalid: ENOENT: no such file or directory, open '/app/librechat.yaml'
  ```

## 步驟

### a. 建立 `librechat.yaml`（專案根目錄）

```yaml
version: 1.3.13

interface:
  skills:
    use: true
    create: true

  bookmarks: false
  prompts: false
  memories: false
  parameters: false
  agents:
    use: false
    create: false
  marketplace:
    use: false
  mcpServers:
    use: false
    create: false
```

### b. 在 `docker-compose.override.yml` 幫 `api` 服務加上掛載

與 [docker-setup.md](./docker-setup.md) 裡的 `container_name` 覆寫合併在同一個 `api:` 區塊：

```yaml
services:
  api:
    container_name: LibreChat-4chat
    volumes:
      - type: bind
        source: ./librechat.yaml
        target: /app/librechat.yaml
```

> `docker-compose.override.yml` 對同一個 service 的 `volumes` 清單是**合併（append）**、不是整個覆蓋，所以不會影響原本 `.env`、`images`、`uploads`、`logs`、`skill` 這幾個既有的掛載。

### c. 重建 `api` 容器套用設定

```bash
docker compose up -d api
```

啟動 log 應會看到權限同步訊息，例如：

```
Updating 'USER' role permission 'PROMPTS' 'USE' from true to: false
Updating 'USER' role permission 'BOOKMARKS' 'USE' from true to: false
Updating 'USER' role permission 'MEMORIES' 'USE' from true to: false
Updating 'USER' role permission 'AGENTS' 'USE' from true to: false
Updating 'USER' role permission 'MCP_SERVERS' 'USE' from true to: false
```

之後重新整理網頁即可看到側欄只剩「新對話」「對話紀錄」「Skills」「附加檔案」四項。

## 常用 `interface.*` 旗標對照（供之後調整）

| 旗標 | 控制的側欄項目 |
|---|---|
| `interface.skills` | Skills 面板 |
| `interface.prompts` | Prompts 面板 |
| `interface.bookmarks` | Bookmarks 面板 |
| `interface.memories` | Memories 面板 |
| `interface.parameters` | Parameters 面板 |
| `interface.agents` | Agent/Assistant 建立器面板 |
| `interface.marketplace.use` | Agents Marketplace（也需要 `agents.use: true` 才會顯示） |
| `interface.mcpServers` | MCP 伺服器管理面板 |
| `interface.fileSearch` | 聊天輸入框的「File Search」工具開關，**不是**附加檔案面板 |
| `interface.temporaryChat` / `interface.webSearch` / `interface.runCode` | 聊天輸入框上方的工具切換，非側欄面板 |

## 與模型端點設定的關聯

側欄裡的「Agent/Assistant 建立器」圖示（`interface.agents.use`）跟模型選單裡的「Agents」端點是同一個開關控制的，兩者無法分開設定，詳見 [model-endpoints.md 的已知限制](./model-endpoints.md#已知限制agents-端點與側欄設定衝突)。
