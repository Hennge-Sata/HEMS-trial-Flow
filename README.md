# HEMS-trial-Flow
HEMS Flow
flowchart TD
    %% 登場人物（レーン）の定義
    subgraph 依頼者
        A[個別評価環境構築依頼を連絡\n※Salesforce評価商談のChatter]
    end

    subgraph CSS
        B[#ext-css-hennge-sales に通知]
    end

    subgraph Biz_Ops["Biz Ops"]
        C([業務開始])
        D[構築日・構築製品を確認]
        E{Chatterでの事前連絡があるか?}
        F{Environment Typeが\nTrialかどうか?}
        G{OukashoRequesterの\nstatusがSUCCESSか?}
        H[One Service Trialレコードを作成\n※クライアント/サーバー両方の場合は別レコード]
        I[管理レポートへの表示を確認]
        J[構築予定日の連絡]
        
        %% エラー・中断プロセス
        E_ERR[依頼者に確認・差し戻し]
        G_ERR[エラー解消を待つか、関係者に確認]
    end

    %% フローの接続
    A --> B
    B --> C
    C --> D
    D --> E
    
    %% 条件分岐1
    E -- No --> E_ERR
    E -- Yes --> F
    
    %% 条件分岐2
    F -- Yes --> G
    F -- No --> H
    
    %% 条件分岐3
    G -- Yes --> H
    G -- No --> G_ERR
    
    %% 後続プロセス
    H --> I
    I --> J
    
    %% 連絡プロセスの分岐
    J --> K[CSS向け: Slack ext-css-hennge-salesのスレッド]
    J --> L[HENNGE向け: 評価商談のChatter配下]
    
    %% 終了
    K --> M([完了])
    L --> M

    %% スタイルの調整
    style C fill:#f9f,stroke:#333,stroke-width:2px
    style M fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#fffdf5,stroke:#ffbc00,stroke-width:2px
    style F fill:#fffdf5,stroke:#ffbc00,stroke-width:2px
    style G fill:#fffdf5,stroke:#ffbc00,stroke-width:2px
    style E_ERR fill:#ffebee,stroke:#c62828,stroke-width:1px
    style G_ERR fill:#ffebee,stroke:#c62828,stroke-width:1px
