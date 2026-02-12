---
name: gog
description: |
  Google Workspace を CLI で操作するスキル。gogcli (gog) コマンドを使い、
  Gmail（メール検索・送信・ラベル管理）、Google Calendar（予定一覧・作成・更新・削除・空き時間確認）、
  Google Drive（ファイル検索・アップロード・ダウンロード・共有）、Google Sheets（セル読み書き・フォーマット）、
  Google Tasks（タスク管理）、Google Docs / Slides（エクスポート・作成）、
  Google Contacts / People（連絡先検索・管理）、Google Chat（メッセージ送信）、
  Google Classroom（コース・課題管理）、Google Groups、Google Keep を操作する。
  「メールを確認」「予定を作成」「ドライブで検索」「スプレッドシートを更新」「タスクを追加」
  「連絡先を検索」「チャットで送信」などのリクエストに対応する。
allowed-tools:
  - Bash(gog *)
  - Bash(which gog)
  - Bash(brew install steipete/tap/gogcli)
  - Read
---

# gogcli スキル

[gogcli](https://github.com/steipete/gogcli) は Google Workspace サービスを統合的に操作する CLI ツール。
`gog <service> <command>` の統一構文で、Gmail / Calendar / Drive / Sheets / Tasks / Docs / Slides / Contacts / People / Chat / Classroom / Groups / Keep を操作できる。

## 前提条件チェック

操作を実行する前に、以下を確認する:

### 1. インストール確認

```bash
which gog
```

未インストールの場合、Homebrew でインストールを提案する:
```bash
brew install steipete/tap/gogcli
```

### 2. 認証確認

```bash
gog auth list
```

アカウントが登録されていない場合、セットアップ手順を案内する。
詳細は [setup.md](reference/setup.md) を参照。

### 3. アカウント設定

`--account` フラグまたは `GOG_ACCOUNT` 環境変数でアカウントを指定する。
ユーザーがアカウントを明示しない場合、`gog auth list` で登録済みアカウントを確認し、どのアカウントを使うか尋ねる。

## 基本原則

### 出力フォーマット

- データの解析・加工が必要な場合は `--json` を付ける
- ユーザーへの表示用にはデフォルト（カラー出力）を使う
- スクリプト連携には `--plain`（TSV 出力）を使う

### コマンド実行の安全性

- **送信・削除・更新** などの副作用のある操作は、必ずユーザーに確認を取ってから実行する
- `--force` フラグは明示的に指示された場合のみ使用する
- メール送信 (`gog gmail send`) は実行前に件名・宛先・本文をユーザーに確認する

### エラーハンドリング

- 認証エラー → `gog auth status` で確認し、再認証を案内
- API エラー → エラーメッセージを解釈し、対処法を提示
- レート制限 → 少し待ってからリトライを提案

### 日時の扱い

- gogcli は相対日時表現をサポート: `today`, `tomorrow`, `monday` 等
- RFC3339 形式: `2025-01-15T09:00:00+09:00`
- 日付のみ: `2025-01-15`（終日イベント用）

## サービス別クイックリファレンス

### Gmail

最頻使用サービス。メール検索、本文取得、送信、ラベル管理を行う。

```bash
# スレッド検索（Gmail クエリ構文）
gog gmail search "is:unread" --max 20
gog gmail search "from:example@gmail.com after:2025/01/01"

# メッセージ本文取得
gog gmail get <messageId>

# メール送信
gog gmail send --to "user@example.com" --subject "件名" --body "本文"

# ラベル一覧
gog gmail labels list
```

詳細: [gmail.md](reference/gmail.md)

### Google Calendar

予定の一覧取得、作成、更新、削除、空き時間確認を行う。

```bash
# 今日の予定
gog calendar events --today

# 今週の予定（全カレンダー）
gog calendar events --week --all

# 予定作成
gog calendar create primary --summary "会議" --from "2025-01-15T10:00:00" --to "2025-01-15T11:00:00"

# 空き時間確認
gog calendar freebusy "user@example.com" --from today --to tomorrow

# 予定の競合検出
gog calendar conflicts --from today --to "next friday"
```

詳細: [calendar.md](reference/calendar.md)

### Google Drive

ファイルの検索、アップロード、ダウンロード、共有管理を行う。

```bash
# ファイル一覧
gog drive ls

# 全文検索
gog drive search "議事録"

# ファイルダウンロード
gog drive download <fileId>

# ファイルアップロード
gog drive upload ./report.pdf --parent <folderId>

# 共有設定
gog drive share <fileId> --type user --role reader --email "user@example.com"
```

詳細: [drive.md](reference/drive.md)

### Google Sheets

スプレッドシートのセル読み書き、フォーマット設定を行う。

```bash
# セル値取得
gog sheets get <spreadsheetId> "Sheet1!A1:D10"

# セル値更新
gog sheets update <spreadsheetId> "Sheet1!A1" "値1" "値2" "値3"

# 行追加
gog sheets append <spreadsheetId> "Sheet1!A:D" "val1" "val2" "val3" "val4"

# 新規作成
gog sheets create "新しいシート"
```

詳細: [sheets.md](reference/sheets.md)

### Google Tasks

タスクリストとタスクの管理を行う。

```bash
# タスクリスト一覧
gog tasks lists list

# タスク一覧
gog tasks list <tasklistId>

# タスク追加
gog tasks add <tasklistId> --title "買い物" --due "2025-01-15"

# タスク完了
gog tasks done <tasklistId> <taskId>
```

詳細: [tasks.md](reference/tasks.md)

### Google Docs / Slides

ドキュメントとスライドのエクスポート、作成を行う。

```bash
# ドキュメントをプレーンテキストで表示
gog docs cat <docId>

# PDF エクスポート
gog docs export <docId> --format pdf

# スライド情報
gog slides info <presentationId>

# スライドを PDF エクスポート
gog slides export <presentationId> --format pdf
```

詳細: [docs-slides.md](reference/docs-slides.md)

### Google Contacts / People

連絡先の検索・作成・管理を行う。

```bash
# 連絡先検索
gog contacts search "田中"

# 連絡先一覧
gog contacts list

# Workspace ディレクトリ検索
gog people search "yamada"

# 自分のプロフィール
gog people me
```

詳細: [contacts.md](reference/contacts.md)

### Google Chat (Workspace 専用)

スペースの一覧取得、メッセージ送信を行う。

```bash
# スペース一覧
gog chat spaces list

# メッセージ送信
gog chat messages send <space> --text "メッセージ本文"

# DM 送信
gog chat dm send user@example.com --text "メッセージ"
```

詳細: [chat.md](reference/chat.md)

### Google Classroom (教育機関向け)

コース、課題、成績の管理を行う。

```bash
# コース一覧
gog classroom courses list

# 名簿取得
gog classroom roster <courseId>

# 課題一覧
gog classroom coursework list <courseId>
```

詳細: [classroom.md](reference/classroom.md)

### その他のサービス

Groups, Keep, Service Account 等の高度な機能。

```bash
# グループ一覧
gog groups list

# Keep メモ一覧（Workspace 専用 / Service Account 必須）
gog keep list

# 認証サービス一覧
gog auth services
```

詳細: [advanced.md](reference/advanced.md)

### クロスサービスワークフロー

複数サービスを横断する便利なワークフロー集。

詳細: [workflows.md](reference/workflows.md)

## グローバルフラグ

すべてのコマンドで使用可能:

| フラグ | 説明 |
|--------|------|
| `--account=EMAIL` | 使用するアカウントを指定 |
| `--client=NAME` | OAuth クライアントを指定 |
| `--json` | JSON 出力（スクリプト向け） |
| `--plain` | TSV 出力（パース向け） |
| `--force` | 確認プロンプトをスキップ |
| `--no-input` | 対話プロンプトを無効化（CI 向け） |
| `--verbose` | 詳細ログ出力 |
| `--color=auto\|always\|never` | カラー出力制御 |

## トラブルシューティング

### 認証エラー: token revoked / expired

```bash
gog auth list --check   # トークン状態確認
gog auth remove you@gmail.com
gog auth add you@gmail.com  # 再認証
```

### API が有効になっていない

Google Cloud Console で該当 API を有効化する:
- Gmail API: `https://console.cloud.google.com/apis/api/gmail.googleapis.com`
- Calendar API: `https://console.cloud.google.com/apis/api/calendar-json.googleapis.com`
- Drive API: `https://console.cloud.google.com/apis/api/drive.googleapis.com`

### コマンドの詳細ヘルプ

```bash
gog <service> --help
gog <service> <command> --help
```

### 設定ファイルの場所

```bash
gog config path
gog config list
```
