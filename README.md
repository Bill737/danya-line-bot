# 達妮婭 LINE Bot（獨立 Render 服務）

這是一個全新的、獨立的 LINE Messaging API Bot。不共用舊 Bot 的程式、Render Service、資料庫或環境變數。

## 功能

- `POST /callback`：驗證 LINE 簽名，快速回應 webhook，再在背景處理文字訊息。
- Gemini 生成繁體中文回覆，角色口吻以達妮婭的慵懶、溫柔與虛質科學背景為核心。
- 每位 LINE 使用者保留最近 12 則訊息，讓對話不會完全失憶。
- `GET /`：Render health check，只顯示必要設定是否存在，不洩漏密鑰。

## Render 建立方式

1. 將這個資料夾推到一個新的 GitHub repository，例如 `danya-line-bot`。
2. 在 Render 選 **New → Web Service**，連到新的 repository；或使用 Blueprint 讀取 `render.yaml`。
3. 確認是全新的 service name，例如 `danya-line-bot`，不要選舊的 `line-bot-webhook`。
4. 在 Render Environment 設定 `LINE_CHANNEL_ACCESS_TOKEN`、`LINE_CHANNEL_SECRET`、`GEMINI_API_KEY`。
5. 部署後，把 LINE Developers Console 的 Webhook URL 設為 `https://<新的-render-host>/callback`，啟用 Use webhook。
6. 先開啟 `https://<新的-render-host>/`，確認 `status` 是 `ok`，再傳一則文字測試。

LINE Token 必須來自這個 Bot 所屬的同一個 Messaging API Channel；不要把 Gemini key 放進 LINE 欄位，也不要在聊天中貼出任何密鑰。

## 本機測試

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
uvicorn main:app --reload
```

Render Free 服務閒置後可能冷啟動；SQLite 放在 `/tmp` 只適合輕量對話記憶，服務重建後可能清空。若要長期記憶，再把 `DB_PATH` 改成持久磁碟或換成 PostgreSQL。
