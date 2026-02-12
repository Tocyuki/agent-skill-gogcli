# Drive コマンドリファレンス

## ファイル一覧

```bash
gog drive ls [flags]
```

デフォルトはルートフォルダの一覧を表示。

## 全文検索

```bash
gog drive search <query> ... [flags]
```

| フラグ | 説明 |
|--------|------|
| `--max=20` | 最大件数 |
| `--page=TOKEN` | ページトークン |

```bash
# キーワード検索
gog drive search "議事録"

# 複数キーワード
gog drive search "Q1 レポート"

# JSON で取得
gog drive search "プレゼン" --json
```

## ファイルメタデータ取得

```bash
gog drive get <fileId>
```

## ファイルダウンロード

```bash
gog drive download <fileId> [flags]
```

Google ドキュメント形式のファイルは自動的にエクスポートされる。

## ファイルコピー

```bash
gog drive copy <fileId> <name> [flags]
```

## ファイルアップロード

```bash
gog drive upload <localPath> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--name=STRING` | ファイル名の上書き |
| `--parent=STRING` | アップロード先フォルダ ID |

```bash
# 基本アップロード
gog drive upload ./report.pdf

# フォルダ指定 + 名前変更
gog drive upload ./report.pdf --parent <folderId> --name "Q1レポート.pdf"
```

## フォルダ作成

```bash
gog drive mkdir <name> [flags]
```

## ファイル削除（ゴミ箱へ移動）

```bash
gog drive delete <fileId>
```

エイリアス: `rm`, `del`

## ファイル移動

```bash
gog drive move <fileId> [flags]
```

## ファイル名変更

```bash
gog drive rename <fileId> <newName>
```

## 共有設定

```bash
gog drive share <fileId> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--anyone` | 公開アクセス |
| `--email=STRING` | 特定ユーザーと共有 |
| `--role="reader"` | 権限: `reader`, `writer` |
| `--discoverable` | 検索で発見可能（anyone/domain のみ） |

```bash
# 特定ユーザーに閲覧権限
gog drive share <fileId> --email "user@example.com" --role reader

# 特定ユーザーに編集権限
gog drive share <fileId> --email "user@example.com" --role writer

# 公開リンク
gog drive share <fileId> --anyone
```

## 共有解除

```bash
gog drive unshare <fileId> <permissionId>
```

## 権限一覧

```bash
gog drive permissions <fileId>
```

## Web URL 表示

```bash
gog drive url <fileId> ...
```

## コメント管理

```bash
gog drive comments <command>
```

## 共有ドライブ一覧

```bash
gog drive drives
```

チームドライブ（共有ドライブ）の一覧を表示。
