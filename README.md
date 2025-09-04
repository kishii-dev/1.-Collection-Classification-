```mermaid
graph TD
    subgraph "1. 収集 (Collection)"
        U1["紙の証憑"] -->|ScanSnap| GD
        U2["外出先のレシート"] -->|スマホカメラ| GD
        U3["メール添付/PCファイル"] -->|"ドラッグ&ドロップ"| GD
    end

    subgraph "2. AIによる自動処理"
        GD["Google Drive"] -- "新規ファイル追加をトリガー" --> GCP["Google Cloud Functions"]
        GCP --> OCR["Google Cloud Vision API<br>(OCR処理)"]
        OCR -- "テキストデータ" --> NLP["分類ロジック<br>(in Cloud Functions)"]
        NLP -- "分類結果" --> GCP
        GCP -- "データ登録/更新" --> NT["Notionデータベース"]
    end

    subgraph "3. 人による確認・修正 (Human Check)"
        NT -- "ステータス='要確認'" --> H["担当者"]
        H -- "内容確認・修正" --> NT
    end

    subgraph "4. 次工程への受け渡し"
        NT -- "ステータス='完了'" --> NEXT["次工程<br>(会計処理/監査対応)"]
    end

    style GD fill:#f9f,stroke:#333,stroke-width:2px
    style NT fill:#f9f,stroke:#333,stroke-width:2px
