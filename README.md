# 跑團報名小幫手

跑友可從 LINE 或一般瀏覽器開啟報名頁，填寫暱稱、預計跑量／配速及選填備註。主辦人在 Google 試算表管理活動與報名名冊。

**[開啟正式報名頁](https://taylorshiehusi.github.io/running-cub/)**

1. 報名前先確認畫面上的活動日期、集合資訊與開放狀態。
2. 填表送出後，查看即時名冊中的報名或候補狀態。現行畫面的送出提示由 iframe 載入事件觸發，實際結果以名冊或主辦人查核為準。
3. 名冊目前會顯示備註，請只填適合公開的內容。

主辦人透過 `Activities.status` 設定 `Open` 或 `Closed`。前端由 GitHub Pages 提供，Google Apps Script（GAS）讀取活動與名冊，並把報名寫入同一份 Google 試算表。

## 專案文件

- [需求與範圍](docs/requirements.md)
- [前後端介面](docs/interfaces.md)
- [流程與狀態](docs/flows.md)
- [部署與驗收](docs/deployment-and-tests.md)
- [歷史審查紀錄](docs/system-review-v1.md)
- [GitHub CLI 登入與開 PR 示範](docs/github-cli-workflow.md)
- [設定與報名資訊流](docs/registration-information-flow.md)
