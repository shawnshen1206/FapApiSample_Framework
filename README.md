# API 使用說明文件

## API 概述
本 API 用於接收聊天記錄與設定，並返回機器人回應的結果。

### URL
Post https://www.sol-idea.com.tw/back/api/CompletionBot/SimplifiedFAQ

### 請求格式
| KEY            | VALUE                |
| -------------- | -------------------- |
| Content-Type   | multipart/form-data  |

### 請求資料範例
#### Layer 1
| KEY            | VALUE                |
| -------------- | -------------------- |
| jsonChatRoomVM | json 字串化後的字典，範例 |
|                | {                   |
|                | "ApiKey": "your_key",|
|                | "ResponseFormat": 0,|
|                | "LogChatLogHistorySN": -1,|
|                | "ChatLogs": [{"HumanContent": "你好阿"}]|
|                | }                   |

#### Layer 2
| KEY                   | VALUE                       |
| --------------------- | --------------------------- |
| ApiKey                | 你的 api key                |
| ResponseFormat        | 要markdown格式填0，Html標籤格式填1            |
| LogChatLogHistorySN   | 想要接續對話紀錄的對話編號，若是開新對話請填 -1 |
| ChatLogs              | 將要送給機器人的字串放在 HumanContent，最少需要3個字，最多200字     |

### curl 請求範例
```
curl https://www.sol-idea.com.tw/back/api/CompletionBot/SimplifiedFAQ --form jsonChatRoomVM="{\"ApiKey\":\"your_key\", \"ResponseFormat\":0, \"LogChatLogHistorySN\":-1,\"ChatLogs\":[{\"HumanContent\": \"你好阿\", }]}"
```
記得換掉your_key
### 回應資料範例
#### Layer 1
| KEY        | VALUE                      |
| ---------- | -------------------------- |
| JsonData   | json 字串化後的資料，範例     |
|            | {                         |
|            | "ApiKey": "your_key",      |
|            | "ResponseFormat": 0,|
|            | "LogChatLogHistorySN": 1234,|
|            | "ChatLogs": [{"HumanContent": "你好阿", "AIContent": "你好！有什麼我可以幫助你的嗎？"}]|
|            | }                         |

#### Layer 2
| KEY                  | VALUE                     |
| -------------------- | ------------------------- |
| ApiKey               | 你的 api key              |
| ResponseFormat       | markdown格式0，Html標籤格式1            |
| LogChatLogHistorySN  | 本次對話的對話編號，如果下次要接著問(保持歷史對話)需要記錄這個編號         |
| ChatLogs             | 機器人的回應會放在 AIContent |

### 回應錯誤處理
使用 Http 400 Bad Request

#### 錯誤格式
| KEY                  | VALUE                     |
| -------------------- | ------------------------- |
| Code                 | 錯誤代碼                  |
| Message              | 錯誤訊息                  |

#### 錯誤列表
| Code                 | Message                   |
| -------------------- | ------------------------- |
| 3001                 | Json字串解析失敗                |
| 3002                 | ChatLogs內容不能為空                  |
| 3003                 | 輸入字串過長(超過200字)             |
| 3004                 | 輸入字串過短(少於3字)                  |
| 4001                 | ApiKey錯誤或不存在                  |
| 4002                 | 問答次數不足                  |
| 4004                 | 對話編號沒有權限或不存在                  |
