<!-- Last synced with README.en.md: 2026-05-12 (947bfa4c40) -->

<p align="center">
  <a href="https://librechat.ai">
    <img src="client/public/assets/logo.svg" height="256">
  </a>
  <h1 align="center">
    <a href="https://librechat.ai">LibreChat</a>
  </h1>
</p>

<p align="center">
  <a href="README.en.md">English</a> ·
  <strong>中文</strong>
</p>

<p align="center">
  <a href="https://discord.librechat.ai"> 
    <img
      src="https://img.shields.io/discord/1086345563026489514?label=&logo=discord&style=for-the-badge&logoWidth=20&logoColor=white&labelColor=000000&color=blueviolet">
  </a>
  <a href="https://www.youtube.com/@LibreChat"> 
    <img
      src="https://img.shields.io/badge/YOUTUBE-red.svg?style=for-the-badge&logo=youtube&logoColor=white&labelColor=000000&logoWidth=20">
  </a>
  <a href="https://docs.librechat.ai"> 
    <img
      src="https://img.shields.io/badge/DOCS-blue.svg?style=for-the-badge&logo=read-the-docs&logoColor=white&labelColor=000000&logoWidth=20">
  </a>
  <a aria-label="Sponsors" href="https://github.com/sponsors/danny-avila">
    <img
      src="https://img.shields.io/badge/SPONSORS-brightgreen.svg?style=for-the-badge&logo=github-sponsors&logoColor=white&labelColor=000000&logoWidth=20">
  </a>
</p>

<p align="center">
<a href="https://railway.com/deploy/librechat-official?referralCode=HI9hWz&utm_medium=integration&utm_source=readme&utm_campaign=librechat">
  <img src="https://railway.com/button.svg" alt="Deploy on Railway" height="30">
</a>
<a href="https://zeabur.com/templates/0X2ZY8">
  <img src="https://zeabur.com/button.svg" alt="Deploy on Zeabur" height="30"/>
</a>
<a href="https://template.cloud.sealos.io/deploy?templateName=librechat">
  <img src="https://raw.githubusercontent.com/labring-actions/templates/main/Deploy-on-Sealos.svg" alt="Deploy on Sealos" height="30">
</a>
</p>

<p align="center">
  <a href="https://www.librechat.ai/docs/translation">
    <img 
      src="https://img.shields.io/badge/dynamic/json.svg?style=for-the-badge&color=2096F3&label=locize&query=%24.translatedPercentage&url=https://api.locize.app/badgedata/4cb2598b-ed4d-469c-9b04-2ed531a8cb45&suffix=%+translated" 
      alt="翻譯進度">
  </a>
</p>


# ✨ 功能

- 🖥️ **UI 與體驗**：受 ChatGPT 啟發，並具備更強的設計與功能。

- 🤖 **AI 模型選擇**：  
  - Anthropic (Claude), AWS Bedrock, OpenAI, Azure OpenAI, Google, Vertex AI, OpenAI Responses API (包含 Azure)
  - [自訂端點 (Custom Endpoints)](https://www.librechat.ai/docs/quick_start/custom_endpoints)：LibreChat 支援任何相容 OpenAI 規範的 API，無需代理。
  - 相容[本地與遠端 AI 服務商](https://www.librechat.ai/docs/configuration/librechat_yaml/ai_endpoints)：
    - Ollama, groq, Cohere, Mistral AI, Apple MLX, koboldcpp, together.ai,
    - OpenRouter, Helicone, Perplexity, ShuttleAI, Deepseek, Qwen 等。

- 🔧 **[程式碼直譯器 (Code Interpreter) API](https://www.librechat.ai/docs/features/code_interpreter)**： 
  - 安全的沙箱執行環境，支援 Python, Node.js (JS/TS), Go, C/C++, Java, PHP, Rust 和 Fortran。
  - 無縫檔案處理：直接上傳、處理並下載檔案。
  - 隱私無憂：完全隔離且安全的執行環境。

- 🔦 **智慧體與工具整合**：  
  - **[LibreChat 智慧體 (Agents)](https://www.librechat.ai/docs/features/agents)**：
    - 無程式碼定制助手：無需編程即可建構專業化的 AI 驅動助手。
    - 智慧體市集：發現並部署社群建構的智慧體。
    - 協作共享：與特定使用者和群組共享智慧體。
    - 靈活且可擴充：支援 MCP 伺服器、工具、檔案搜尋、程式碼執行等。
    - [Skills](https://www.librechat.ai/docs/features/skills)：建立可重複使用的 `SKILL.md` 指令包，用於手動、自動或始終啟用的智慧體工作流程。
    - [Subagents](https://www.librechat.ai/docs/features/subagents)：將專門任務委派給擁有獨立上下文視窗的隔離子智慧體執行。
    - 相容自訂端點、OpenAI, Azure, Anthropic, AWS Bedrock, Google, Vertex AI, Responses API 等。
    - [支援模型上下文協定 (MCP)](https://modelcontextprotocol.io/clients#librechat) 用於工具呼叫。

- 🔍 **網頁搜尋**：  
  - 搜尋網際網路並擷取相關資訊以增強 AI 上下文。
  - 結合搜尋提供商、內容爬蟲和結果重新排序，確保最佳擷取效果。
  - **可自訂 Jina 重新排序**：設定自訂 Jina API URL 用於重新排序服務。
  - **[了解更多 →](https://www.librechat.ai/docs/features/web_search)**

- 🪄 **支援程式碼 Artifacts 的生成式 UI**：  
  - [程式碼 Artifacts](https://youtu.be/GfTj7O4gmd0?si=WJbdnemZpJzBrJo3) 允許在對話中直接建立 React 元件、HTML 頁面和 Mermaid 圖表。

- 🎨 **影像生成與編輯**：
  - 使用 [GPT-Image-1](https://www.librechat.ai/docs/features/image_gen#1--openai-image-tools-recommended) 進行文生圖與圖生圖。
  - 支援 [DALL-E (3/2)](https://www.librechat.ai/docs/features/image_gen#2--dalle-legacy), [Stable Diffusion](https://www.librechat.ai/docs/features/image_gen#3--stable-diffusion-local), [Flux](https://www.librechat.ai/docs/features/image_gen#4--flux) 或任何 [MCP 伺服器](https://www.librechat.ai/docs/features/image_gen#5--model-context-protocol-mcp)。
  - 根據提示詞生成驚豔的視覺效果，或透過指令精修現有影像。

- 💾 **預設與上下文管理**：  
  - 建立、儲存並分享自訂預設。
  - 在對話中隨時切換 AI 端點和預設。
  - 編輯、重新提交並透過對話分支繼續訊息。
  - 建立並與特定使用者和群組共享提示詞。
  - [訊息與對話分岔 (Fork)](https://www.librechat.ai/docs/features/fork) 以實現進階上下文控制。

- 💬 **多模態與檔案互動**：  
  - 使用 Claude 3, GPT-4.5, GPT-4o, o1, Llama-Vision 和 Gemini 上傳並分析影像 📸。  
  - 支援透過自訂端點、OpenAI, Azure, Anthropic, AWS Bedrock 和 Google 進行檔案對話 🗃️。

- 🌎 **多語言 UI**：
  - English, 中文 (簡體), 中文 (繁體), العربية, Deutsch, Español, Français, Italiano
  - Polski, Português (PT), Português (BR), Русский, 日本語, Svenska, 한국어, Tiếng Việt
  - Türkçe, Nederlands, עברית, Català, Čeština, Dansk, Eesti, فارسی
  - Suomi, Magyar, Հայերեն, Bahasa Indonesia, ქართული, Latviešu, ไทย, ئۇيغۇرچە

- 🧠 **推理 UI**：  
  - 針對 DeepSeek-R1 等思維鏈/推理 AI 模型的動態推理 UI。

- 🎨 **可自訂介面**：  
  - 可自訂的下拉選單和介面，同時適配進階使用者和初學者。

- 🌊 **[可恢復串流 (Resumable Streams)](https://www.librechat.ai/docs/features/resumable_streams)**：
  - 永不遺失回應：AI 回應在連線中斷後自動重新連線並繼續。
  - 多分頁與多裝置同步：在多個分頁開啟同一對話，或在另一裝置上繼續。
  - 生產級可靠性：支援從單機部署到基於 Redis 的水平擴充。

- 🗣️ **語音與音訊**：  
  - 透過語音轉文字和文字轉語音實現免持對話。  
  - 自動傳送並播放音訊。  
  - 支援 OpenAI, Azure OpenAI 和 Elevenlabs。

- 📥 **匯入與匯出對話**：  
  - 從 LibreChat, ChatGPT, Chatbot UI 匯入對話。  
  - 將對話匯出為截圖、Markdown、文字、JSON。

- 🔍 **搜尋與探索**：  
  - 搜尋所有訊息和對話。

- 👥 **多使用者與安全存取**：
  - 支援 OAuth2, LDAP 和電子郵件登入的多使用者安全認證。
  - 內建審核系統和 Token 消耗管理工具。

- ⚙️ **設定與部署**：  
  - 支援代理、反向代理、Docker 及多種部署選項。  
  - 使用 [S3 與 CloudFront](https://www.librechat.ai/docs/configuration/cdn/cloudfront) 獲得穩定的媒體連結、邊緣分發、簽章 Cookie 和安全下載。
  - 可完全本地執行或部署在雲端。

- 📖 **開源與社群**：  
  - 完全開源且在公眾監督下開發。  
  - 社群驅動的開發、支援與回饋。

[查看我們的文件了解更多功能詳情](https://docs.librechat.ai/) 📚

## 🪶 LibreChat：全方位的 AI 對話平台

LibreChat 是一個自架的 AI 對話平台，在一個注重隱私的統一介面中整合了所有主流 AI 服務商。

除了對話功能外，LibreChat 還提供 AI 智慧體、模型上下文協定 (MCP) 支援、Artifacts、程式碼直譯器、自訂操作、對話搜尋，以及企業級多使用者認證。

開源、積極開發中，專為重視 AI 基礎設施自主可控的使用者而建構。

---

## 🐳 本機 Docker 快速開始

官方完整文件託管在外部網站 [docs.librechat.ai](https://docs.librechat.ai)，以下僅整理**在本機以 Docker 啟動本專案**所需的最少步驟，方便快速對照。

1. **準備環境變數**：確認專案根目錄已有 `.env`（可從 `.env.example` 複製），並至少設定：
   - `PORT`：LibreChat API/前端服務的 port（預設 `3080`）。
   - `DOMAIN_CLIENT` / `DOMAIN_SERVER`：需與 `PORT` 一致，例如 `PORT=3080` 時設為 `http://localhost:3080`。
   - `ADMIN_PANEL_SESSION_SECRET`：使用內建 admin panel 時**必填**，長度需 ≥ 32 字元，否則該容器會直接拒絕啟動並不斷重啟。可用 `openssl rand -hex 32` 產生。
   - `ADMIN_PANEL_PORT`：admin panel 對外的 port（預設 `3000`）。

2. **啟動服務**：
   ```bash
   docker compose up -d
   ```

3. **檢查容器狀態**（確認全部為 `Up`／`healthy`）：
   ```bash
   docker compose ps
   ```

### 若本機已安裝過其他 LibreChat（port／容器名稱衝突）

`docker-compose.yml` 中每個服務都有固定的 `container_name`（`LibreChat`、`chat-mongodb`、`admin-panel` 等）。如果同一台機器上已經跑了另一份 LibreChat 安裝，啟動時會因為 port 或容器名稱重複而失敗。此時**不要直接修改 `docker-compose.yml`**，改用 `docker-compose.override.yml` 覆寫，例如：

```yaml
# docker-compose.override.yml
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

再搭配 `.env` 把對外 port 改掉（例如 `PORT=3088`、`ADMIN_PANEL_PORT=3089`，並同步更新 `DOMAIN_CLIENT`/`DOMAIN_SERVER`），即可與既有安裝並存。可用 `docker compose config` 先驗證沒有重複的 port／容器名稱，再執行 `docker compose up -d`。

**本專案目前的實際設定**（已套用上述調整，避免與本機另一套 LibreChat 衝突）：
- LibreChat：<http://localhost:3088>
- Admin Panel：<http://localhost:3089>

---

## 🌐 資源

**GitHub 儲存庫：**
  - **RAG API:** [github.com/danny-avila/rag_api](https://github.com/danny-avila/rag_api)
  - **網站:** [github.com/LibreChat-AI/librechat.ai](https://github.com/LibreChat-AI/librechat.ai)

**其他：**
  - **官方網站:** [librechat.ai](https://librechat.ai)
  - **說明文件:** [librechat.ai/docs](https://librechat.ai/docs)
  - **部落格:** [librechat.ai/blog](https://librechat.ai/blog)

---

## 📝 更新日誌

造訪發布頁面和更新日誌以了解最新動態：
- [發布頁面 (Releases)](https://github.com/danny-avila/LibreChat/releases)
- [更新日誌 (Changelog)](https://www.librechat.ai/changelog)

**⚠️ 在更新前請務必查看[更新日誌](https://www.librechat.ai/changelog)以了解破壞性變更。**

---

## ⭐ Star 歷史

<p align="center">
  <a href="https://star-history.com/#danny-avila/LibreChat&Date">
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=danny-avila/LibreChat&type=Date&theme=dark" onerror="this.src='https://api.star-history.com/svg?repos=danny-avila/LibreChat&type=Date'" />
  </a>
</p>
<p align="center">
  <a href="https://trendshift.io/repositories/4685" target="_blank" style="padding: 10px;">
    <img src="https://trendshift.io/api/badge/repositories/4685" alt="danny-avila%2FLibreChat | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/>
  </a>
  <a href="https://runacap.com/ross-index/q1-24/" target="_blank" rel="noopener" style="margin-left: 20px;">
    <img style="width: 260px; height: 56px" src="https://runacap.com/wp-content/uploads/2024/04/ROSS_badge_white_Q1_2024.svg" alt="ROSS Index - 2024年第一季度成長最快的開源新創公司 | Runa Capital" width="260" height="56"/>
  </a>
</p>

---

## ✨ 貢獻

歡迎任何形式的貢獻、建議、錯誤報告和修正！

對於新功能、元件或擴充，請在送出 PR 前開啟 issue 進行討論。

如果您想幫助我們將 LibreChat 翻譯成您的母語，我們非常歡迎！改善翻譯不僅能讓全球使用者更輕鬆地使用 LibreChat，還能提升整體使用者體驗。請查看我們的[翻譯指南](https://www.librechat.ai/docs/translation)。

---

## 💖 感謝所有貢獻者

<a href="https://github.com/danny-avila/LibreChat/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=danny-avila/LibreChat" />
</a>

---

## 🎉 特別鳴謝

感謝 [Locize](https://locize.com) 提供的翻譯管理工具，支援 LibreChat 的多語言功能。

<p align="center">
  <a href="https://locize.com" target="_blank" rel="noopener noreferrer">
    <img src="https://github.com/user-attachments/assets/d6b70894-6064-475e-bb65-92a9e23e0077" alt="Locize Logo" height="50">
  </a>
</p>
