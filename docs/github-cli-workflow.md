# 用 GitHub CLI 登入與開 PR

以跑團報名專案的「更新部署驗收文件並開 PR」為例，說明共用登入流程如何接上專案更新。範例 repo 為 `TaylorShiehUSI/running-cub`，PR 的基準分支為 `main`。

| 責任 | 範例中的做法 |
|---|---|
| 共用登入流程 | 在實際執行指令的背景終端檢查 `gh auth status`。已有有效登入時沿用，需要登入時由終端發起裝置授權。 |
| 專案更新流程 | 核對遠端 repo、基準分支及權限。更新文件後，推送工作分支並開 PR。 |
| GitHub 權限 | GitHub 按照帳號、憑證、repo 權限與分支規則判斷操作是否可執行。 |

## 流程圖

```mermaid
flowchart TD
    A["核對 running-cub 與基準分支 main"] --> B{"背景終端已有預期帳號的有效登入？"}
    B -->|"是"| F["核對 repo 權限與 Git 推送憑證"]
    B -->|"未登入或登入失效"| C["確認設定可寫及憑證儲存方式"]
    C --> D["終端發起裝置登入，使用者在 GitHub 授權"]
    D --> E["gh 儲存憑證，另開終端驗證可沿用"]
    E --> F
    B -->|"帳號不符"| X["核對或切換帳號後重查"]
    X --> B
    F --> G["更新部署驗收文件，檢查差異"]
    G --> H["提交並推送工作分支"]
    H --> I["開 PR 到 main"]
    I --> J["回查 PR 網址及提交 SHA"]
```

## 時序圖

```mermaid
sequenceDiagram
    actor U as 使用者
    participant A as Codex
    participant T as 背景終端
    participant G as GitHub

    U->>A: 更新部署驗收文件並開 PR
    A->>T: 核對 repo 與基準分支，檢查 gh 登入

    alt 已以預期帳號登入
        T-->>A: 回傳帳號與登入狀態
    else 尚未登入或登入失效
        A->>T: 確認設定可寫，執行 gh auth login
        T->>G: 發起裝置授權
        G-->>T: 回傳一次性裝置碼與驗證網址
        T-->>A: 提供裝置碼與驗證網址
        A-->>U: 顯示裝置碼與驗證網址
        U->>G: 在瀏覽器輸入裝置碼並授權
        T->>G: 查詢授權結果
        G-->>T: 授權成功後回傳登入憑證
        Note over T: gh 儲存憑證，另開終端確認登入可沿用
        T-->>A: 回傳已驗證的帳號
    end

    A->>T: 核對 repo 權限與 Git 推送憑證
    A->>T: 修改部署文件、核對差異並提交
    T->>G: git push 推送工作分支
    G-->>T: 回傳推送結果
    T->>G: gh pr create 開 PR 到 main
    G-->>T: 回傳 PR 網址
    T->>G: gh pr view 回查狀態與提交 SHA
    G-->>T: 回傳 PR 狀態與提交 SHA
    T-->>A: 回傳核對結果
    A-->>U: 提供 PR 網址與提交 SHA
```

瀏覽器顯示授權成功後，背景終端還要成功保存憑證，才能讓下一個命令沿用。以 HTTPS 推送時，Git 也需要能取得憑證。開 PR 後，可用 PR 網址、基準分支及提交 SHA 核對結果。

[跑團部署與驗收文件](deployment-and-tests.md)
