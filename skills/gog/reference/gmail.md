# Gmail コマンドリファレンス

エイリアス: `gog gmail`, `gog mail`, `gog email`

## スレッド検索

Gmail クエリ構文でスレッドを検索。

```bash
gog gmail search "<query>" [flags]
```

| フラグ | 説明 |
|--------|------|
| `--max=10` | 最大件数 |
| `--page=TOKEN` | ページトークン |
| `--oldest` | 最初のメッセージ日付を表示 |
| `-z, --timezone=STRING` | 出力タイムゾーン（IANA 名） |
| `--local` | ローカルタイムゾーン |

### Gmail クエリ構文の例

```bash
# 未読メール
gog gmail search "is:unread"

# 特定の送信者から
gog gmail search "from:user@example.com"

# 日付範囲
gog gmail search "after:2025/01/01 before:2025/02/01"

# 添付ファイル付き
gog gmail search "has:attachment"

# ラベル指定
gog gmail search "label:important"

# 件名に含む
gog gmail search "subject:会議"

# 複合条件
gog gmail search "from:boss@company.com is:unread after:2025/01/01"

# 最大20件取得
gog gmail search "is:unread" --max 20
```

## メッセージ検索

スレッドではなくメッセージ単位で検索。

```bash
gog gmail messages search "<query>" [flags]
```

## メッセージ取得

```bash
gog gmail get <messageId> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--format="full"` | 出力形式: `full`, `metadata`, `raw` |
| `--headers=STRING` | メタデータヘッダー（`--format=metadata` 用） |

```bash
# 全文取得
gog gmail get <messageId>

# メタデータのみ
gog gmail get <messageId> --format metadata --headers "From,Subject,Date"

# 生データ
gog gmail get <messageId> --format raw
```

## 添付ファイルダウンロード

```bash
gog gmail attachment <messageId> <attachmentId>
```

## スレッド操作

### スレッド取得

```bash
gog gmail thread get <threadId> [flags]
```

スレッド内の全メッセージを表示。

### スレッドの添付ファイル一覧

```bash
gog gmail thread attachments <threadId>
```

### スレッドのラベル変更

```bash
gog gmail thread modify <threadId> [flags]
```

## Web URL 表示

```bash
gog gmail url <threadId> ...
```

## メール送信

```bash
gog gmail send [flags]
```

| フラグ | 説明 |
|--------|------|
| `--to=STRING` | 宛先（カンマ区切り、必須） |
| `--cc=STRING` | CC |
| `--bcc=STRING` | BCC |
| `--subject=STRING` | 件名（必須） |
| `--body=STRING` | 本文（プレーンテキスト） |
| `--body-file=STRING` | 本文ファイルパス（`-` で stdin） |
| `--body-html=STRING` | HTML 本文 |
| `--reply-to-message-id=STRING` | 返信先メッセージ ID |
| `--thread-id=STRING` | スレッド内返信 |
| `--reply-all` | 全員に返信 |
| `--reply-to=STRING` | Reply-To ヘッダー |
| `--attach=FILE,...` | 添付ファイル（複数可） |
| `--from=STRING` | 送信元アドレス（send-as エイリアス） |
| `--track` | 開封トラッキング |
| `--track-split` | 受信者別トラッキング |

```bash
# 基本送信
gog gmail send --to "user@example.com" --subject "件名" --body "本文"

# CC/BCC 付き
gog gmail send --to "a@ex.com" --cc "b@ex.com" --bcc "c@ex.com" --subject "件名" --body "本文"

# ファイル添付
gog gmail send --to "user@example.com" --subject "資料送付" --body "添付をご確認ください" --attach ./report.pdf

# 返信
gog gmail send --reply-to-message-id <messageId> --body "了解しました"

# スレッド内で全員に返信
gog gmail send --thread-id <threadId> --reply-all --body "ありがとうございます"

# HTML メール
gog gmail send --to "user@example.com" --subject "ニュースレター" --body-html "<h1>タイトル</h1><p>本文</p>"
```

## 下書き操作

### 下書き一覧

```bash
gog gmail drafts list
```

### 下書き取得

```bash
gog gmail drafts get <draftId>
```

### 下書き作成

```bash
gog gmail drafts create [flags]
```

`send` と同じフラグが使用可能（`--to`, `--subject`, `--body` 等）。

### 下書き更新

```bash
gog gmail drafts update <draftId> [flags]
```

### 下書き送信

```bash
gog gmail drafts send <draftId>
```

### 下書き削除

```bash
gog gmail drafts delete <draftId>
```

## ラベル操作

### ラベル一覧

```bash
gog gmail labels list
```

### ラベル詳細（件数含む）

```bash
gog gmail labels get <labelIdOrName>
```

### ラベル作成

```bash
gog gmail labels create "新しいラベル"
```

### スレッドのラベル変更

```bash
gog gmail labels modify <threadId> ... [flags]
```

## バッチ操作

### メッセージの一括ラベル変更

```bash
gog gmail batch modify <messageId> ... [flags]
```

### メッセージの一括削除（永久削除）

```bash
gog gmail batch delete <messageId> ...
```

## 設定・管理

### フィルター

```bash
gog gmail settings filters <command>
```

### 委任（Delegate）

```bash
gog gmail settings delegates <command>
```

### 転送設定

```bash
gog gmail settings forwarding <command>
gog gmail settings autoforward <command>
```

### Send-As 設定

```bash
gog gmail settings sendas <command>
```

### 不在通知

```bash
gog gmail settings vacation <command>
```

### Gmail Watch（Pub/Sub プッシュ通知）

```bash
gog gmail settings watch <command>
```

## 履歴

```bash
gog gmail history [flags]
```

## 開封トラッキング

```bash
gog gmail track <command>
```

`gog gmail send --track` で送信したメールの開封状況を追跡。
Cloudflare Worker バックエンドのセットアップが必要。
