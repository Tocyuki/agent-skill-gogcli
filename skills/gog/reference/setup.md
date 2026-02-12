# セットアップガイド

gogcli の認証・設定手順。すべてのサービス操作の前提条件。

## インストール

### Homebrew（推奨）

```bash
brew install steipete/tap/gogcli
```

### インストール確認

```bash
gog --version
```

## OAuth 認証のセットアップ

### 1. Google Cloud Console で OAuth クライアントを作成

1. [Google Cloud Console](https://console.cloud.google.com/apis/credentials) を開く
2. プロジェクトを作成（未作成の場合）
3. 必要な API を有効化:
   - [Gmail API](https://console.cloud.google.com/apis/api/gmail.googleapis.com)
   - [Calendar API](https://console.cloud.google.com/apis/api/calendar-json.googleapis.com)
   - [Drive API](https://console.cloud.google.com/apis/api/drive.googleapis.com)
   - [Sheets API](https://console.cloud.google.com/apis/api/sheets.googleapis.com)
   - [Tasks API](https://console.cloud.google.com/apis/api/tasks.googleapis.com)
   - [People API](https://console.cloud.google.com/apis/api/people.googleapis.com)
   - [Chat API](https://console.cloud.google.com/apis/api/chat.googleapis.com)（Workspace のみ）
   - [Classroom API](https://console.cloud.google.com/apis/api/classroom.googleapis.com)（教育機関のみ）
   - [Cloud Identity API](https://console.cloud.google.com/apis/api/cloudidentity.googleapis.com)（Groups 用）
4. [OAuth 同意画面](https://console.cloud.google.com/auth/branding) を設定
5. テストモードの場合、[テストユーザー](https://console.cloud.google.com/auth/audience) を追加
6. [OAuth クライアント](https://console.cloud.google.com/auth/clients) を作成:
   - アプリケーションの種類: 「デスクトップ アプリ」
   - JSON ファイルをダウンロード

### 2. 認証情報の保存

```bash
gog auth credentials set ~/Downloads/client_secret_xxx.json
```

保存済みの認証情報一覧:
```bash
gog auth credentials list
```

### 3. アカウント認証

```bash
gog auth add you@gmail.com
```

ブラウザが開き、OAuth 認証フローが始まる。

#### サービスの選択

デフォルトでは `user` サービスが認証される。特定のサービスのみ認証:

```bash
# すべてのサービスを認証
gog auth add you@gmail.com --services all

# 特定のサービスのみ
gog auth add you@gmail.com --services gmail,calendar,drive
```

#### 読み取り専用スコープ

```bash
gog auth add you@gmail.com --readonly
```

#### Drive スコープの選択

```bash
gog auth add you@gmail.com --drive-scope full      # フルアクセス（デフォルト）
gog auth add you@gmail.com --drive-scope readonly   # 読み取り専用
gog auth add you@gmail.com --drive-scope file       # ファイル単位
```

### 4. 認証テスト

```bash
gog auth status
gog gmail labels list
```

## ヘッドレス / リモートサーバー認証

ブラウザのないサーバーでの認証:

```bash
# ステップ 1: 認証 URL を表示（ローカルのブラウザで開く）
gog auth add you@gmail.com --services user --remote --step 1

# ステップ 2: ブラウザのリダイレクト URL を貼り付け
gog auth add you@gmail.com --services user --remote --step 2 --auth-url 'http://localhost:1/?code=...&state=...'
```

## 複数アカウント管理

### アカウント一覧

```bash
gog auth list
gog auth list --check   # トークンの有効性も確認
```

### アカウントの指定

```bash
# フラグで指定
gog --account you@gmail.com gmail labels list

# 環境変数で指定
export GOG_ACCOUNT=you@gmail.com
```

### アカウントエイリアス

```bash
gog auth alias set personal you@gmail.com
gog --account personal gmail labels list
```

## 複数 OAuth クライアント

異なるプロジェクト・組織用に複数の OAuth クライアントを使用:

```bash
# 名前付きクライアントの登録
gog --client work auth credentials set ~/Downloads/work-client.json
gog --client work auth add you@company.com

# ドメインマッピング（自動選択）
gog --client work auth credentials set ~/Downloads/work.json --domain company.com
```

クライアント選択順序:
1. `--client` フラグ / `GOG_CLIENT` 環境変数
2. `account_clients` 設定（メール → クライアント）
3. `client_domains` 設定（ドメイン → クライアント）
4. ドメイン名の認証情報ファイル
5. `default`

## Service Account (Workspace 専用)

ドメイン全体の委任を使用する Workspace 向け認証:

```bash
# Service Account の設定
gog auth service-account set --key /path/to/sa-key.json --email admin@company.com

# Keep 用の Service Account
gog auth keep --key /path/to/sa-key.json admin@company.com
```

## 設定管理

```bash
# 設定ファイルの場所
gog config path

# 設定一覧
gog config list

# 設定キー一覧
gog config keys

# 設定値の取得・変更
gog config get <key>
gog config set <key> <value>
gog config unset <key>
```

## Keyring バックエンド

トークンの保存先を変更:

```bash
# 現在のバックエンド確認
gog auth status

# バックエンド変更
gog auth keyring <backend>
```

## アカウントの削除

```bash
gog auth remove you@gmail.com
```

## コマンド制限（サンドボックス）

特定のコマンドのみ許可:

```bash
gog --enable-commands gmail,calendar gmail labels list
```
