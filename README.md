# 社内情報特化型生成AI検索アプリ (RAG System)

本プロジェクトは、社内のドキュメント（PDF, DOCX, CSV, TXT）を学習させ、生成AI（OpenAI GPT-4o-mini）が業務上の質問に対して根拠に基づいた回答を行う **RAG (Retrieval-Augmented Generation)** アプリケーションです。

## 📂 フォルダ構成と役割

プロジェクトは機能ごとにモジュール化されており、`main.py` を中心に各ファイルが連携して動作します。

| ファイル・フォルダ名 | 役割 |
| :--- | :--- |
| **● main.py** | **アプリのメインエントリーポイント。** 処理の全体像を制御します。 |
| **● initialize.py** | アプリ起動時に一度だけ実行。ログ設定、ベクターストア(Chroma)の作成。 |
| **● components.py** | UI表示に特化した関数を管理（チャット画面、ラジオボタンなど）。 |
| **● utils.py** | LLMからの回答取得、アイコン取得など、表示以外のロジックを管理。 |
| constants.py | 固定値（モデル名、定数、タイトル名など）の一括管理。 |
| .env | OpenAI APIキーなどの秘匿情報の管理。 |
| requirements.txt | アプリ動作に必要なパッケージ一覧（`pip install` 用）。 |
| data/ | RAGの参照先データ（PDF, CSV等）を格納。 |
| .chroma/ | 作成されたベクターストアの実体データ（自動生成）。 |
| logs/ | 実行ログ（application.log）が出力されるフォルダ。 |

-----

## 🚀 アプリケーションの全体像

### 1\. 処理の流れ

1.  **初期化**: `initialize.py` が走り、`data/` 内の文書をベクトル化して `.chroma/` に保存します。
2.  **待機**: `main.py` が起動し、Streamlit UIが表示されます。
3.  **入力**: ユーザーがチャット欄から質問を送信します。
4.  **検索と生成**: `utils.py` を介して関連文書を検索し、GPT-4o-miniが回答を生成します。
5.  **表示**: `components.py` が回答と参照元のありかを画面に整形して出力します。

### 2\. 回答モード

  - **社内文書検索**: 入力内容に最も関連性が高い文書のありか（PDFのページ番号含む）を検索します。
  - **社内問い合わせ**: 社内文書の文脈（Context）に基づき、具体的な回答を生成します。

-----

## 🛠 改修・追加のポイント（Tips）

  - **設定値を変えたい場合**: `constants.py` の数値を変更してください（CHUNK\_SIZE, RETRIEVAL\_Kなど）。
  - **画面のデザインを直したい場合**: `components.py` の関数内の `st.markdown` や `st.success` などを修正してください。
  - **新しいファイル形式に対応させる場合**: `constants.py` の `SUPPORTED_EXTENSIONS` にLoaderを追加し、`initialize.py` を更新します。

-----

## 📦 セットアップと実行

1.  **環境構築**
    ```bash
    python -m venv env
    source env/bin/activate  # Windows: env\Scripts\activate
    pip install -r requirements.txt
    ```
2.  **APIキーの設定**
    `.env` ファイルに `OPENAI_API_KEY=YOUR_KEY` を記述。
3.  **起動**
    ```bash
    streamlit run main.py
    ```

-----

## 💡 デバッグとログ

  - 変数の中身を確認したい場合は、`print()` 関数（ターミナル出力）または `logger.info()` を活用し、`logs/application.log` を確認してください。
  - 動作が不安定な場合は、`.chroma` フォルダを削除してインデックスを再作成（アプリの再起動）してください。
