# 1.-Collection-Classification-
証憑や取引データを収集し、体系的に分類
# 証憑収集・分類 自動化プロジェクト (pbc-automation)

## 概要

経理・監査業務における証憑や取引データ（PBC）の収集・分類プロセスを自動化するためのプロジェクトです。
Google Drive, Google Cloud Vision API, Notion APIを連携させ、手作業によるアップロードと確認作業を最小限に抑えることを目的とします。

## アーキテクチャ

```mermaid
graph TD
    subgraph A[1. 収集 (Collection)]
        U1[紙の証憑] -->|ScanSnap| GD
        U2[外出先のレシート] -->|スマホカメラ| GD
        U3[メール添付/PCファイル] -->|ドラッグ&ドロップ| GD
    end

    subgraph B[2. AIによる自動処理]
        GD[Google Drive] -- 新規ファイル追加をトリガー --> GCP[Google Cloud Functions]
        GCP --> OCR[Google Cloud Vision API<br>(OCR処理)]
        OCR -- テキストデータ --> NLP[分類ロジック<br>(in Cloud Functions)]
        NLP -- 分類結果 --> GCP
        GCP -- データ登録/更新 --> NT[Notionデータベース]
    end

    subgraph C[3. 人による確認・修正 (Human Check)]
        NT -- ステータス='要確認' --> H[担当者]
        H -- 内容確認・修正 --> NT
    end

    subgraph D[4. 次工程への受け渡し]
        NT -- ステータス='完了' --> NEXT[次工程<br>(会計処理/監査対応)]
    end

    style GD fill:#f9f,stroke:#333,stroke-width:2px
    style NT fill:#f9f,stroke:#333,stroke-width:2px
```

## セットアップ方法

### 1. 必要なライブラリのインストール
```bash
pip install -r requirements.txt
```

### 2. 環境変数の設定
`.env.example` をコピーして `.env` ファイルを作成し、必要なAPIキーやIDを設定してください。

```bash
cp .env.example .env
# .env ファイルを編集して各値を設定
```

### 3. Google Cloud / NotionのAPI設定
（ここに各APIの認証情報取得手順へのリンクなどを記載）

## 使い方

（ここにスクリプトの実行方法などを記載）
