# 報名流程與活動狀態

## 跑友報名

```mermaid
sequenceDiagram
    actor Runner as 跑友
    participant Web as 報名頁
    participant GAS as Apps Script
    participant Sheet as Google 試算表
    Runner->>Web: 從 LINE 或瀏覽器開啟
    Web->>GAS: 查詢活動與名冊
    GAS->>Sheet: 讀取活動與報名
    GAS-->>Web: 回傳活動狀態與名冊
    Runner->>Web: 填表送出
    Web->>GAS: POST register
    GAS->>Sheet: 讀取該活動狀態
    alt 活動開放且資料有效
        GAS->>Sheet: 新增正取或候補，或更新原列
        GAS-->>Web: 回傳後端結果
    else 活動截止或資料無效
        GAS-->>Web: 回傳錯誤
    end
    Web-->>Runner: iframe 載入後顯示送出提示
```

GAS 在收到請求時檢查活動。若主辦人在跑友填表期間關閉報名，GAS 仍會拒絕新增和修改。現行 iframe 提示可能仍顯示成功。

## 主辦人開關報名

```mermaid
stateDiagram-v2
    Open --> Closed: 主辦人改表截止
    Closed --> Open: 主辦人改表重新開放
```

1. 在 Activities 確認活動 ID 和資料列。
2. 設定 status 為 Open 或 Closed。
3. 重開報名頁對照場次和狀態。若另有 Open 場次，預設頁可能改顯示另一場。
4. 若需群組公告，由主辦人確認內容和公開範圍，再手動發送。
