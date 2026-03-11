# 如何將通知分配到不同的 Discord Thread

## 什麼是 Discord Thread？
Thread（討論串）是 Discord 文字頻道內的子對話，可以讓你在同一個頻道中組織不同主題的訊息。

## 設定步驟

### 1. 開啟 Discord 開發者模式
1. 打開 Discord → **使用者設定** ⚙️
2. 進入 **進階 (Advanced)**
3. 開啟 **開發者模式 (Developer Mode)** 開關

### 2. 建立或找到你的 Thread
在你的 Discord 文字頻道中：
- 如果還沒有 thread，可以建立新的（點擊訊息旁的 `+` 或使用 `/thread` 指令）
- 為每個 Threads 帳號建立專屬的 thread（例如：「投資理財」、「股市快訊」）

### 3. 複製 Thread ID
1. 右鍵點擊你要使用的 **thread 名稱**
2. 選擇 **複製 ID (Copy ID)**
3. 會得到一串數字，例如：`1234567890123456789`

### 4. 更新 sources.json
在每個 source 中加入 `thread_id` 欄位：

```json
[
  {
    "id": "make_investment_easy",
    "platform": "threads",
    "name": "投資理財分享",
    "url": "https://www.threads.com/@make_investment_easy",
    "enabled": true,
    "check_interval_minutes": 60,
    "parser_type": "threads_public_profile",
    "thread_id": "1234567890123456789"  ← 貼上你複製的 Thread ID
  },
  {
    "id": "stock_glee",
    "platform": "threads",
    "name": "股市快訊",
    "url": "https://www.threads.com/@stock_glee",
    "enabled": true,
    "check_interval_minutes": 60,
    "parser_type": "threads_public_profile",
    "thread_id": "9876543210987654321"  ← 不同的 Thread ID
  }
]
```

### 5. 測試設定
```powershell
python app.py
```

## 注意事項

### ✅ 設定為 `null` 或不設定
如果不想使用 thread，設定為 `null` 或省略該欄位：
```json
"thread_id": null  // 發送到主頻道
```

### ✅ 混合使用
- 某些 source 發送到 thread
- 某些 source 發送到主頻道
- 完全沒問題！

### ⚠️ 權限要求
確保你的 Webhook 所在的 Bot 或頻道有權限發送訊息到該 thread。

### ⚠️ Thread 必須存在
Thread ID 必須是已經存在的 thread，否則發送會失敗。

## 範例配置

```json
[
  {
    "id": "account_a",
    "name": "主要帳號",
    "url": "https://www.threads.com/@account_a",
    "enabled": true,
    "thread_id": null  ← 發送到主頻道
  },
  {
    "id": "account_b",
    "name": "次要帳號",
    "url": "https://www.threads.com/@account_b",
    "enabled": true,
    "thread_id": "1234567890123456"  ← 發送到 Thread A
  },
  {
    "id": "account_c",
    "name": "測試帳號",
    "url": "https://www.threads.com/@account_c",
    "enabled": true,
    "thread_id": "6543210987654321"  ← 發送到 Thread B
  }
]
```

## 除錯

如果發送失敗，檢查：
1. Thread ID 是否正確（19 位數字）
2. Thread 是否仍然存在（沒有被刪除）
3. Webhook 是否有權限訪問該 thread
4. 查看 log 中的錯誤訊息
