# クロスサービスワークフロー集

複数の Google Workspace サービスを組み合わせた実用的なワークフロー。

## メール添付ファイルを Drive に保存

Gmail の添付ファイルを取得し、Drive にアップロードする。

```bash
# 1. メールを検索
gog gmail search "has:attachment from:client@example.com" --max 5

# 2. メッセージ詳細を取得（添付ファイル情報含む）
gog gmail get <messageId> --json

# 3. 添付ファイルをダウンロード
gog gmail attachment <messageId> <attachmentId>

# 4. Drive にアップロード
gog drive upload ./downloaded-file.pdf --parent <folderId> --name "クライアント資料.pdf"
```

## カレンダー予定の参加者にメール送信

特定の予定の参加者に一斉メールを送る。

```bash
# 1. 予定を検索
gog calendar events --query "週次定例" --today --json

# 2. 予定の詳細から参加者リストを取得
gog calendar event primary <eventId> --json

# 3. 参加者にメール送信
gog gmail send --to "attendee1@ex.com,attendee2@ex.com" \
  --subject "週次定例のアジェンダ" \
  --body "本日の議題は以下の通りです..."
```

## Sheets データからメール一括送信

スプレッドシートの連絡先リストを使ってメールを送信。

```bash
# 1. スプレッドシートからデータ取得
gog sheets get <spreadsheetId> "Sheet1!A2:C100" --json

# 2. データを確認してから各行のメールアドレスに送信
gog gmail send --to "user@example.com" --subject "お知らせ" --body "..."
```

## 予定作成 + 資料共有

会議を作成し、関連資料を参加者に共有する。

```bash
# 1. Drive で資料を検索
gog drive search "Q1レビュー資料"

# 2. 資料のURLを取得
gog drive url <fileId>

# 3. 参加者に共有設定
gog drive share <fileId> --email "user@example.com" --role reader

# 4. カレンダーに予定作成（資料URL付き）
gog calendar create primary \
  --summary "Q1 レビュー会議" \
  --from "2025-01-20T14:00:00" --to "2025-01-20T15:00:00" \
  --attendees "user@example.com,user2@example.com" \
  --with-meet \
  --description "資料: https://drive.google.com/file/d/<fileId>/view" \
  --attachment "https://drive.google.com/file/d/<fileId>/view"
```

## タスクとカレンダーの連携

タスクの期日をカレンダーで確認しながら管理。

```bash
# 1. タスクリスト確認
gog tasks lists list

# 2. タスク一覧（JSON で期日を確認）
gog tasks list <tasklistId> --json

# 3. 今日の予定と照合
gog calendar events --today

# 4. 空き時間にタスク作業用ブロックを作成
gog calendar create primary \
  --summary "タスク: レポート作成" \
  --from "2025-01-15T15:00:00" --to "2025-01-15T16:00:00" \
  --event-type focus-time
```

## 日次レポートの自動生成

今日の活動をまとめてスプレッドシートに記録。

```bash
# 1. 今日の予定を取得
gog calendar events --today --json

# 2. 今日の送受信メール件数を確認
gog gmail search "after:today" --json

# 3. 完了タスクを確認
gog tasks list <tasklistId> --json

# 4. スプレッドシートに記録を追加
gog sheets append <spreadsheetId> "DailyLog!A:E" \
  "2025-01-15" "会議3件" "メール12件" "タスク5件完了" "特記事項なし"
```

## ドキュメント作成 → 共有 → 通知

新しいドキュメントを作成し、チームに共有・通知する。

```bash
# 1. ドキュメント作成
gog docs create "議事録 - 2025/01/15 チームMTG"

# 2. 共有設定
gog drive share <docId> --email "team@company.com" --role writer

# 3. チームにメールで通知
gog gmail send --to "team@company.com" \
  --subject "議事録を共有しました" \
  --body "本日のチームMTGの議事録を作成しました。\nhttps://docs.google.com/document/d/<docId>/edit"

# 4. Chat でも通知（Workspace の場合）
gog chat messages send <spaceName> --text "議事録を共有しました: https://docs.google.com/document/d/<docId>/edit"
```

## 空き時間を探して会議を設定

複数の参加者の空き時間を確認し、最適な時間に会議を設定。

```bash
# 1. 参加者の空き時間を確認
gog calendar freebusy "a@company.com,b@company.com,c@company.com" \
  --from "next monday" --to "next friday"

# 2. 空き時間を確認後、予定を作成
gog calendar create primary \
  --summary "プロジェクト打合せ" \
  --from "2025-01-20T10:00:00" --to "2025-01-20T11:00:00" \
  --attendees "a@company.com,b@company.com,c@company.com" \
  --with-meet \
  --send-updates all
```

## 連絡先情報を Sheets にエクスポート

連絡先リストをスプレッドシートにまとめる。

```bash
# 1. 連絡先を取得
gog contacts list --json

# 2. 新しいスプレッドシートを作成
gog sheets create "連絡先一覧 2025"

# 3. ヘッダー行を設定
gog sheets update <spreadsheetId> "Sheet1!A1:D1" "名前" "メール" "電話" "会社"

# 4. データを追加
gog sheets append <spreadsheetId> "Sheet1!A:D" "田中太郎" "tanaka@ex.com" "090-xxxx" "A社"
```

## 週次サマリーの作成

1週間の活動をまとめる。

```bash
# 1. 今週のカレンダー予定
gog calendar events --week --all --json

# 2. 今週のメールスレッド
gog gmail search "after:monday before:saturday" --max 50

# 3. タスクの進捗
gog tasks list <tasklistId> --json

# 4. サマリーを Docs に作成
gog docs create "週次サマリー - 2025/01/13週"
```
