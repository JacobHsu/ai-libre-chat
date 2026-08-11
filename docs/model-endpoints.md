# `librechat.yaml` 模型端點設定教學（Azure OpenAI）

## 概觀

本文件說明如何透過 `librechat.yaml` 的 `endpoints` 區塊，設定聊天室可選用的模型端點，並以 Azure OpenAI 為例。這跟左側面板顯示項目是**不同的設定區塊**，互不影響（僅 Agents 端點例外，見下方〈已知限制〉）：

- 左側面板（哪些圖示顯示/隱藏）：[sidebar-panel.md](./sidebar-panel.md)
- Docker 安裝與 port／容器名稱衝突處理：[docker-setup.md](./docker-setup.md)

官方文件：[librechat.ai/docs/configuration/librechat_yaml/ai_endpoints](https://www.librechat.ai/docs/configuration/librechat_yaml/ai_endpoints)

## 設定步驟

### 1. `.env`：啟用的端點清單與金鑰

```dotenv
ENDPOINTS=openAI,agents,custom,azureOpenAI
AZURE_OPENAI_API_KEY=<你的 Azure OpenAI API Key>
```

- `ENDPOINTS`：決定模型選單裡會出現哪些端點類型（未列出的端點類型即使有其他設定也不會顯示）。
- `AZURE_OPENAI_API_KEY`：`librechat.yaml` 裡 `${AZURE_OPENAI_API_KEY}` 這個變數實際讀取的值。

#### 顯示／隱藏特定端點

`ENDPOINTS` 是逗號分隔的清單，**只要把某個端點類型從清單移除，它就不會出現在模型選單**，不需要動 `librechat.yaml`。例如要隱藏 OpenAI：

```dotenv
# 隱藏前
ENDPOINTS=openAI,agents,custom,azureOpenAI

# 隱藏後（拿掉 openAI）
ENDPOINTS=agents,custom,azureOpenAI
```

可用的端點類型（依 LibreChat 內建支援）：`openAI`、`assistants`、`azureOpenAI`、`azureAssistants`、`agents`、`google`、`anthropic`、`bedrock`、`custom`。

改完 `.env` 存檔後，**要重啟容器才會生效**（`.env` 是 bind mount 的檔案，改內容不會觸發 `docker compose up -d` 自動重建，見〈[套用設定](#3-套用設定)〉）：

```bash
docker restart LibreChat-4chat
```

> 這個檔案是**唯一**要改的地方 — `ENDPOINTS` 只存在於 `.env`，`librechat.yaml` 沒有對應設定。

### 2. `librechat.yaml`：Azure OpenAI 端點設定

跟側欄用的 `interface` 區塊放在同一個檔案裡，但是獨立的頂層區塊：

```yaml
endpoints:
  azureOpenAI:
    groups:
      - group: '<自訂群組名稱>'
        apiKey: '${AZURE_OPENAI_API_KEY}'
        instanceName: '<Azure OpenAI 資源名稱>'
        version: '2025-04-01-preview'
        models:
          <介面顯示的模型名稱>:
            deploymentName: '<Azure 上實際的部署名稱>'
```

| 欄位 | 說明 |
|---|---|
| `group` | 自訂識別名稱，同一個 `azureOpenAI.groups` 陣列可放多組（例如對應不同 Azure 資源） |
| `instanceName` | Azure OpenAI 資源名稱（`https://<instanceName>.openai.azure.com`） |
| `version` | Azure OpenAI API 版本 |
| `models` 底下的 key | 使用者在介面上看到的模型名稱，可自訂 |
| `deploymentName` | Azure 上實際的部署名稱，不一定要跟模型 key 同名 |

### 3. 套用設定

`.env` 和 `librechat.yaml` 是 bind mount 進 `api` 容器的檔案，內容變更不會觸發 `docker compose up -d` 自動重建容器（Compose 偵測不到檔案內容差異），需要手動重啟讓 Node 行程重新讀取：

```bash
docker restart LibreChat-4chat
```

啟動 log 應會印出解析後的 `azureOpenAI` 設定內容，且沒有錯誤訊息。

## 已知限制：Agents 端點與側欄設定衝突

`ENDPOINTS` 清單即使列了 `agents`，只要 [sidebar-panel.md](./sidebar-panel.md) 裡 `interface.agents.use` 設成 `false`（為了精簡側欄），前端 [`useEndpoints.ts`](../client/src/hooks/Endpoint/useEndpoints.ts) 就會把 `agents` 從模型選單過濾掉，畫面上不會出現「Agents」這個端點選項 —— 這兩個開關綁在一起，無法只開放模型選單而不顯示側欄的 Agent 建立器圖示。若要開放 Agents 端點，需把 `interface.agents.use` 改回 `true`，側欄也會連帶恢復 Agent 建立器圖示。

## Custom 端點：目前已設定 Ollama

`ENDPOINTS` 裡的 `custom` 對應 `librechat.yaml` 的 `endpoints.custom` 陣列，目前只設定了 Ollama：

```yaml
endpoints:
  custom:
    - name: 'Ollama'
      apiKey: 'ollama' # Ollama 不驗證金鑰，填任意非空字串即可
      baseURL: 'http://host.docker.internal:11434/v1/'
      models:
        default: ['llama3:latest']
        fetch: true # 啟動時向 Ollama 動態抓已下載的模型清單
```

`host.docker.internal` 能連到主機是因為 `docker-compose.yml` 的 `api` 服務本來就有設定 `extra_hosts: host.docker.internal:host-gateway`，不需要額外調整。

要再加其他 OpenAI 相容供應商（Groq、OpenRouter、Helicone 等），需要：

1. 在 `.env` 補上該供應商的 API Key
2. 在 `librechat.yaml` 的 `endpoints.custom` 陣列**加一筆**新的設定（保留 Ollama 那筆，`custom` 是陣列可以放多個），範例可參考 `librechat.example.yaml`
