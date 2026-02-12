# Chat コマンドリファレンス

Google Chat API。**Google Workspace アカウント専用**。

## スペース操作

### スペース一覧

```bash
gog chat spaces list [flags]
```

参加しているスペース（チャットルーム）の一覧を表示。

### スペース検索

```bash
gog chat spaces find <displayName> [flags]
```

表示名でスペースを検索。

```bash
gog chat spaces find "プロジェクトA"
```

### スペース作成

```bash
gog chat spaces create <displayName> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--member=EMAIL,...` | メンバー（メールアドレスまたは `users/...`、カンマ区切りまたは複数指定可） |

```bash
# スペース作成
gog chat spaces create "新プロジェクト"

# メンバー付きで作成
gog chat spaces create "チームA" --member "a@company.com" --member "b@company.com"
```

## メッセージ操作

### メッセージ一覧

```bash
gog chat messages list <space> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--max=50` | 最大件数 |
| `--page=TOKEN` | ページトークン |
| `--order=STRING` | 並び順（例: `createTime desc`） |
| `--thread=STRING` | スレッドでフィルタ（`spaces/.../threads/...`） |
| `--unread` | 未読メッセージのみ |

```bash
# スペースのメッセージ一覧
gog chat messages list <spaceName>

# 未読のみ
gog chat messages list <spaceName> --unread

# 特定スレッド
gog chat messages list <spaceName> --thread "spaces/.../threads/..."

# 最新20件
gog chat messages list <spaceName> --max 20
```

### メッセージ送信

```bash
gog chat messages send <space> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--text=STRING` | メッセージ本文（必須） |
| `--thread=STRING` | スレッドに返信（`spaces/.../threads/...`） |

```bash
# メッセージ送信
gog chat messages send <spaceName> --text "お疲れ様です"

# スレッドに返信
gog chat messages send <spaceName> --text "了解しました" --thread "spaces/.../threads/..."
```

## スレッド操作

```bash
gog chat threads <command>
```

## ダイレクトメッセージ (DM)

### DM 送信

```bash
gog chat dm send <email> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--text=STRING` | メッセージ本文（必須） |
| `--thread=STRING` | スレッドに返信 |

```bash
# DM 送信
gog chat dm send user@company.com --text "お疲れ様です、少しお時間いただけますか？"
```

### DM スペースの検索・作成

```bash
gog chat dm space <email>
```

指定したユーザーとの DM スペースを検索し、なければ作成する。
