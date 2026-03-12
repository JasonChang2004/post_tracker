# Social Media Monitor Bot v2.0

## 功能特性

- 🔔 自動追蹤社交媒體帳號最新貼文
- 📬 即時通知到 Discord
- 🔄 重試機制與錯誤處理
- ⏰ 可自訂檢查間隔
- 📊 健康檢查通知

## 支援平台

### **v2.0 新增功能**
- ✅ **Facebook 公開頁面追蹤**
- ✅ Threads 公開帳號追蹤

## 安裝

### 依賴套件

```bash
pip install -r requirements.txt
python -m playwright install chromium
```

### 環境變數設定

創建 `.env` 檔案並設定：

```env
DISCORD_WEBHOOK_URL=your_webhook_url_here
```

## 設定

編輯 `config/sources.json` 來新增要追蹤的來源：

### Threads 帳號範例

```json
{
  "id": "make_investment_easy",
  "platform": "threads",
  "name": "make_investment_easy",
  "url": "https://www.threads.com/@make_investment_easy",
  "enabled": true,
  "check_interval_minutes": 60,
  "parser_type": "threads_public_profile",
  "thread_id": 1481470478865666088
}
```

### Facebook 頁面範例

```json
{
  "id": "8tbow_facebook",
  "platform": "facebook",
  "name": "8TBOW",
  "url": "https://www.facebook.com/8TBOW",
  "enabled": true,
  "check_interval_minutes": 60,
  "parser_type": "facebook_public_page",
  "thread_id": null
}
```

## 使用方法

```bash
python app.py
```

## 目前追蹤的 Facebook 頁面

1. [8TBOW](https://www.facebook.com/8TBOW)
2. [Unclestocknotess](https://www.facebook.com/Unclestocknotess)

## 資料夾結構

```
.
├── app.py                  # 主程式
├── requirements.txt        # Python 依賴
├── config/
│   └── sources.json       # 來源設定
└── data/
    └── state.json         # 狀態記錄
```

## 更新日誌

### v2.0 (2026-03-12)
- ✨ 新增 Facebook 平台支援
- ✨ 實作 FacebookFetcher 類別
- ✨ 修改 SourceLoader 支援多平台
- ✨ 修改 BotRunner 根據平台選擇對應的 fetcher
- 📦 新增 8TBOW 和 Unclestocknotess 兩個 Facebook 頁面追蹤

### v1.0
- ✅ Threads 平台追蹤功能
- ✅ Discord 通知整合
- ✅ 重試機制與錯誤處理

自動監控 Threads 帳號的新貼文，並透過 Discord Webhook 發送通知。

## ✨ 功能特色

- 🔄 **自動監控**：定期檢查指定的 Threads 帳號新貼文
- 📢 **Discord 通知**：精美 Embed 格式推送至頻道或 Thread
- 💾 **智能去重**：避免重複通知相同貼文
- 🖼️ **圖片支援**：自動提取並顯示貼文圖片
- ⏱️ **靈活排程**：可自訂檢查間隔時間
- 🤖 **GitHub Actions**：雲端自動執行，無需本地運行
- 🔁 **自動重試**：網路失敗自動重試，提高穩定性
- ⚠️ **錯誤警報**：批次失敗時自動發送錯誤通知
- 🏥 **健康檢查**：24 小時無新貼文時發送運作確認
- 🌏 **台北時間**：所有時間自動轉換為台北時區顯示

## 環境需求

- Python 3.11+
- Playwright（自動安裝 Chromium 瀏覽器）
- Discord Webhook URL

## 本地開發設定

### 1. 安裝依賴

```bash
pip install -r requirements.txt
playwright install chromium
```

### 2. 設定環境變數

複製 `.env.example` 並重命名為 `.env`，填入您的 Discord Webhook URL：

```bash
cp .env.example .env
```

編輯 `.env`：
```
DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/your_webhook_url
```

### 3. 設定監控來源

編輯 `config/sources.json`，新增或修改要監控的 Threads 帳號：

```json
[
  {
    "id": "unique_source_id",
    "platform": "threads",
    "name": "顯示名稱",
    "url": "https://www.threads.com/@username",
    "enabled": true,
    "check_interval_minutes": 60,
    "parser_type": "threads_public_profile"
  }
]
```

### 4. 執行程式

```bash
python app.py
```

## GitHub Actions 部署

### 1. 設定 Secret

在您的 GitHub repository 設定中：
1. 前往 **Settings** → **Secrets and variables** → **Actions**
2. 點擊 **New repository secret**
3. 名稱：`DISCORD_WEBHOOK_URL`
4. 值：貼上您的 Discord Webhook URL
5. 點擊 **Add secret**

### 2. 啟用 Workflow

程式碼推送到 GitHub 後，workflow 會自動啟用：
- 預設每小時執行一次（整點）
- 可手動觸發執行：**Actions** → **Threads Monitor Bot** → **Run workflow**

### 3. 調整執行頻率

編輯 `.github/workflows/monitor.yml` 中的 cron 表達式：

```yaml
schedule:
  - cron: '0 * * * *'  # 每小時
  # - cron: '*/30 * * * *'  # 每 30 分鐘
  # - cron: '0 */2 * * *'  # 每 2 小時
```

## 設定說明

### sources.json 參數

- `id`: 唯一識別碼
- `platform`: 固定填 `"threads"`
- `name`: 來源顯示名稱（會顯示在 Discord 通知中）
- `url`: Threads 帳號的完整 URL
- `enabled`: `true` 啟用 / `false` 停用
- `check_interval_minutes`: 檢查間隔（分鐘）
- `parser_type`: 固定填 `"threads_public_profile"`
- `thread_id`: （選填）Discord Thread ID，將通知發送到特定討論串

## 🔧 進階功能

### Discord Thread 路由

可將不同來源的通知發送到同一頻道的不同討論串：

1. 開啟 Discord **開發者模式**（設定 → 進階）
2. 右鍵點擊 Thread → **複製討論串 ID**
3. 在 `sources.json` 設定 `thread_id`

詳見 [HOW_TO_USE_THREADS.md](HOW_TO_USE_THREADS.md)

### 錯誤警報

當超過 50% 來源抓取失敗時，自動發送 🔴 紅色警報到 Discord。

### 健康檢查

連續 24 小時無新貼文時，發送 🟢 綠色健康檢查通知。

### 自動重試

- Threads 抓取：2 次重試，3 秒間隔
- Discord 通知：3 次重試，2 秒間隔
- 支援指數退避策略

## 📚 文件

- [版本更新說明](CHANGELOG_v1.0.md) - v1.0 優化內容
- [新增帳號教學](HOW_TO_ADD_ACCOUNTS.md)
- [Thread 路由設定](HOW_TO_USE_THREADS.md)

## 🐛 問題排查

### 檢查方式
1. GitHub Actions → 執行記錄
2. Discord 錯誤警報通知
3. `data/state.json` → `last_error_message`

### 常見問題
- **無通知**：確認 `sources.json` 的 `enabled: true`
- **重複通知**：等待下次執行，state.json 會自動同步
- **抓取失敗**：帳號可能為私人，或網路暫時異常（會自動重試）

## 📝 版本

**目前版本**：1.0  
**更新日期**：2026年3月12日

**v1.0 重點**：
- Playwright 資源管理
- 重試機制與錯誤通知
- 日誌精簡 70%
- 台北時間統一顯示

詳見 [CHANGELOG_v1.0.md](CHANGELOG_v1.0.md)

## 專案結構

```
.
├── app.py                      # 主程式
├── requirements.txt            # Python 依賴
├── .env                        # 環境變數（不會提交到 Git）
├── .env.example                # 環境變數範例
├── config/
│   └── sources.json           # 監控來源設定
├── data/
│   └── state.json             # 運行狀態（自動生成）
└── .github/
    └── workflows/
        └── monitor.yml        # GitHub Actions 設定
```

## 運作原理

1. 讀取 `config/sources.json` 取得要監控的帳號列表
2. 檢查每個來源是否到達檢查間隔時間
3. 使用 Playwright 抓取 Threads 公開頁面
4. 解析 HTML 提取貼文 ID、內容、發布時間
5. 比對 `data/state.json` 過濾已通知的貼文
6. 將新貼文透過 Discord Webhook 發送通知
7. 更新狀態檔案記錄已通知的貼文

## 注意事項

- Threads 可能有速率限制，建議檢查間隔設定為 30 分鐘以上
- GitHub Actions 每次執行時間約 15-30 秒
- 首次執行會將最近的貼文全部通知一次
- state.json 會記錄最近 50 則已通知的貼文

## 故障排除

### Discord 通知失敗
- 檢查 Webhook URL 是否正確
- 確認 Webhook 沒有被刪除或停用

### 無法抓取貼文
- 確認 Threads 帳號是公開的
- 檢查網路連線是否正常
- Threads 頁面結構可能有更新，需要調整解析邏輯

### GitHub Actions 執行失敗
- 檢查 Secret 是否設定正確
- 查看 Actions 執行日誌找出錯誤原因

## 授權

MIT License
