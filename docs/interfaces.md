# 前後端介面

基準為 main 的 index.html 與正式 GAS 第 8 版。範例皆使用假資料。

## 查詢活動與名冊

前端使用 JSONP 查詢：GET /exec?callback=renderRosterCallback&_=時間戳。GAS 也支援純 JSON。查詢可指定 activity_id。前端查預設場次，由後往前選 Open 場次，無開放場次時以最後一筆活動為顯示基礎。

成功回應包含 status: "success"、activity、count、participants。前端用 participants.length 顯示人數，包含候補。

| 回應物件 | 欄位 |
|---|---|
| activity | activity_id、name、session_title、status、max_people、event_date、meeting_time、location、slogan |
| participants[] | nickname、running_plan、note、status、updated_at |

## 送出報名

表單使用 URL 編碼 POST /exec，回應載入隱藏 iframe。

| 欄位 | 範例 | 用途 |
|---|---|---|
| action | register | 報名動作 |
| activity_id | demo-01 | 由活動查詢填入，指定場次 |
| line_user_id | USER_DEMO | 前端提供的識別值，與活動 ID 組成更新依據 |
| nickname | 測試跑友 | 必填，前端上限 20 字 |
| running_plan | 8 km／6:00 | 必填，前端上限 50 字 |
| note | 測試備註 | 選填，前端上限 100 字 |

缺省 action 沿用 register。空白 activity_id 目前會搜尋最新開放場次。正常送出應使用指定活動 ID。無 LINE 身分時使用 USER_ 開頭的臨時值。

| 結果 | GAS 回應與資料 |
|---|---|
| 新增正取／候補 | status: "success"，報名狀態為報名成功或候補 |
| 同場既有識別值 | status: "success"、action: "updated"，更新原列 |
| 截止或資料無效 | status: "error" 並附錯誤訊息 |
| close_activity | status: "error"、message: "未知的請求動作"，隔離測試確認無寫入 |

現行前端依 iframe 載入顯示送出提示。實際結果需查看後端回應或資料列。

## 試算表對照

- Activities 保存場次、活動描述、status 與 max_people。
- Registrations 保存場次、識別值、暱稱、跑量／配速、備註、狀態及時間。公開名冊回傳欄位如上。
- Notifications 預留公告紀錄，現行流程未啟用群組自動公告。
