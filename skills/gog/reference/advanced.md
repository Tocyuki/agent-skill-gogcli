# 高度な機能リファレンス

Groups, Keep, Service Account, Config, Time 等の追加機能。

## Google Groups (Workspace)

### グループ一覧

```bash
gog groups list [flags]
```

自分が所属するグループの一覧を表示。

### グループメンバー一覧

```bash
gog groups members <groupEmail> [flags]
```

```bash
gog groups members team@company.com
```

## Google Keep (Workspace 専用)

**Service Account + ドメイン全体の委任が必要。**

### メモ一覧

```bash
gog keep list [flags]
```

| フラグ | 説明 |
|--------|------|
| `--service-account=STRING` | Service Account JSON ファイルパス |
| `--impersonate=STRING` | 代理対象メールアドレス |

### メモ詳細

```bash
gog keep get <noteId> [flags]
```

### メモ検索

```bash
gog keep search <query> [flags]
```

クライアントサイドでテキスト検索を実行。

### 添付ファイルダウンロード

```bash
gog keep attachment <attachmentName> [flags]
```

### Keep のセットアップ

```bash
# Service Account を設定
gog auth keep --key /path/to/sa-key.json admin@company.com
```

## Service Account (Workspace)

ドメイン全体の委任を使用する認証方式。

### Service Account の設定

```bash
gog auth service-account set --key /path/to/sa-key.json --email admin@company.com
```

### Service Account 情報の確認

```bash
gog auth service-account <command>
```

## 認証管理の詳細

### 対応サービスとスコープ一覧

```bash
gog auth services [flags]
```

各サービスが要求する OAuth スコープを表示。

### トークン管理

```bash
gog auth tokens <command>
```

保存済みのリフレッシュトークンを管理。

### アカウントエイリアス

```bash
gog auth alias set <alias> <email>
gog auth alias list
gog auth alias remove <alias>
```

### アカウント管理画面

```bash
gog auth manage
```

エイリアス: `login`。ブラウザでアカウント管理画面を開く。

### Keyring バックエンド

```bash
gog auth keyring [<backend>]
gog auth status
```

## 設定管理

### 設定ファイルのパス

```bash
gog config path
```

### 設定一覧

```bash
gog config list
```

### 設定キー一覧

```bash
gog config keys
```

### 設定値の取得・変更・削除

```bash
gog config get <key>
gog config set <key> <value>
gog config unset <key>
```

## ローカル時刻ユーティリティ

```bash
gog time now [flags]
```

スクリプト・エージェント向けの時刻表示。

## シェル補完

```bash
gog completion <shell>
```

対応シェル: `bash`, `zsh`, `fish`

## コマンド制限（サンドボックス）

エージェントやサンドボックス環境で特定のコマンドのみ許可:

```bash
gog --enable-commands gmail,calendar gmail labels list
```

`--enable-commands` でカンマ区切りのトップレベルコマンドを指定すると、それ以外のコマンドは実行不可になる。
