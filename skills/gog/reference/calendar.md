# Calendar コマンドリファレンス

## カレンダー一覧

```bash
gog calendar calendars
```

## 予定一覧

```bash
gog calendar events [<calendarId>] [flags]
```

`calendarId` 省略時は `primary`。

| フラグ | 説明 |
|--------|------|
| `--from=STRING` | 開始時刻（RFC3339, 日付, 相対: `today`, `tomorrow`, `monday`） |
| `--to=STRING` | 終了時刻 |
| `--today` | 今日のみ |
| `--tomorrow` | 明日のみ |
| `--week` | 今週 |
| `--days=N` | 今後 N 日間 |
| `--week-start=""` | 週の開始曜日（`sun`, `mon` 等） |
| `--max=10` | 最大件数 |
| `--page=TOKEN` | ページトークン |
| `--query=STRING` | テキスト検索 |
| `--all` | 全カレンダーから取得 |
| `--fields=STRING` | 返すフィールド（カンマ区切り） |
| `--weekday` | 曜日列を追加 |
| `--private-prop-filter=STRING` | プライベート拡張プロパティでフィルタ |
| `--shared-prop-filter=STRING` | 共有拡張プロパティでフィルタ |

```bash
# 今日の予定
gog calendar events --today

# 今週の全カレンダー予定
gog calendar events --week --all

# 今後7日間
gog calendar events --days 7

# 特定カレンダーの予定
gog calendar events user@example.com --from today --to "next friday"

# テキスト検索
gog calendar events --query "会議" --from today --days 30

# JSON で取得
gog calendar events --today --json
```

## 予定検索

```bash
gog calendar search <query> [flags]
```

## 予定の詳細取得

```bash
gog calendar event <calendarId> <eventId>
```

## 予定の作成

```bash
gog calendar create <calendarId> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--summary=STRING` | タイトル |
| `--from=STRING` | 開始時刻（RFC3339） |
| `--to=STRING` | 終了時刻（RFC3339） |
| `--description=STRING` | 説明 |
| `--location=STRING` | 場所 |
| `--attendees=STRING` | 参加者メール（カンマ区切り） |
| `--all-day` | 終日イベント |
| `--rrule=STRING` | 繰り返しルール |
| `--reminder=STRING` | リマインダー（例: `popup:30m`, `email:1d`） |
| `--event-color=STRING` | イベントカラー ID (1-11) |
| `--visibility=STRING` | 公開設定: `default`, `public`, `private`, `confidential` |
| `--transparency=STRING` | 表示: `busy`(opaque), `free`(transparent) |
| `--send-updates=STRING` | 通知: `all`, `externalOnly`, `none` |
| `--with-meet` | Google Meet リンクを作成 |
| `--guests-can-invite` | ゲストが他のユーザーを招待可能 |
| `--guests-can-modify` | ゲストがイベントを変更可能 |
| `--guests-can-see-others` | ゲストが他のゲストを閲覧可能 |
| `--attachment=URL` | 添付ファイル URL |
| `--private-prop=KEY=VALUE` | プライベート拡張プロパティ |
| `--shared-prop=KEY=VALUE` | 共有拡張プロパティ |

```bash
# 基本的な予定作成
gog calendar create primary --summary "チームMTG" --from "2025-01-15T10:00:00" --to "2025-01-15T11:00:00"

# 参加者付き + Google Meet
gog calendar create primary --summary "1on1" --from "2025-01-15T14:00:00" --to "2025-01-15T14:30:00" \
  --attendees "user@example.com" --with-meet

# 終日イベント
gog calendar create primary --summary "祝日" --from "2025-01-01" --to "2025-01-02" --all-day

# 繰り返し予定（毎週月曜）
gog calendar create primary --summary "週次定例" --from "2025-01-06T09:00:00" --to "2025-01-06T10:00:00" \
  --rrule "RRULE:FREQ=WEEKLY;BYDAY=MO"

# 場所・説明付き
gog calendar create primary --summary "クライアント打合せ" \
  --from "2025-01-15T13:00:00" --to "2025-01-15T14:00:00" \
  --location "東京オフィス 会議室A" --description "Q1 レビュー"
```

## 予定の更新

```bash
gog calendar update <calendarId> <eventId> [flags]
```

`create` と同様のフラグに加え:

| フラグ | 説明 |
|--------|------|
| `--add-attendee=STRING` | 参加者を追加（既存を維持） |
| `--scope="all"` | 繰り返し予定の適用範囲: `single`, `future`, `all` |
| `--original-start=STRING` | インスタンスの元の開始時刻（`scope=single,future` で必須） |

```bash
# タイトル変更
gog calendar update primary <eventId> --summary "新しいタイトル"

# 時間変更
gog calendar update primary <eventId> --from "2025-01-15T11:00:00" --to "2025-01-15T12:00:00"

# 参加者追加
gog calendar update primary <eventId> --add-attendee "new@example.com"

# 繰り返し予定のこのイベント以降を変更
gog calendar update primary <eventId> --scope future --original-start "2025-01-15T09:00:00"
```

## 予定の削除

```bash
gog calendar delete <calendarId> <eventId>
```

## 空き時間確認

```bash
gog calendar freebusy <calendarIds> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--from=STRING` | 開始時刻 |
| `--to=STRING` | 終了時刻 |

```bash
# 特定ユーザーの空き時間
gog calendar freebusy "user@example.com" --from today --to tomorrow

# 複数ユーザー
gog calendar freebusy "a@ex.com,b@ex.com" --from "2025-01-15" --to "2025-01-16"
```

## 招待への応答

```bash
gog calendar respond <calendarId> <eventId> [flags]
```

## 新しい時間の提案

```bash
gog calendar propose-time <calendarId> <eventId>
```

ブラウザで開くための URL を生成。

## 予定の競合検出

```bash
gog calendar conflicts [flags]
```

```bash
# 今週の競合
gog calendar conflicts --from today --to "next friday"
```

## 特殊イベントタイプ

### フォーカスタイム

```bash
gog calendar focus-time --from "2025-01-15T09:00:00" --to "2025-01-15T12:00:00"
```

### 不在（Out of Office）

```bash
gog calendar out-of-office --from "2025-01-20" --to "2025-01-22"
```

エイリアス: `ooo`

### 勤務場所

```bash
gog calendar working-location --from "2025-01-15" --to "2025-01-16" --type home
gog calendar working-location --from "2025-01-15" --to "2025-01-16" --type office
```

エイリアス: `wl`

## カレンダーカラー

```bash
gog calendar colors
```

利用可能なカラー ID の一覧を表示。

## ACL（アクセス制御）

```bash
gog calendar acl <calendarId>
```

## サーバー時刻

```bash
gog calendar time
```

## Workspace ユーザー一覧

```bash
gog calendar users
```

## チームカレンダー

Google Group のメンバー全員の予定を表示:

```bash
gog calendar team <group-email>
```
