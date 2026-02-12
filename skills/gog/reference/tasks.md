# Tasks コマンドリファレンス

## タスクリスト操作

### タスクリスト一覧

```bash
gog tasks lists list
```

すべてのタスクリストを表示。タスク操作には `tasklistId` が必要なので、まずこのコマンドで ID を確認する。

## タスク一覧

```bash
gog tasks list <tasklistId> [flags]
```

```bash
# 基本一覧
gog tasks list <tasklistId>

# JSON で取得
gog tasks list <tasklistId> --json
```

## タスク詳細

```bash
gog tasks get <tasklistId> <taskId>
```

## タスク追加

```bash
gog tasks add <tasklistId> [flags]
```

エイリアス: `create`

| フラグ | 説明 |
|--------|------|
| `--title=STRING` | タスクタイトル（必須） |
| `--notes=STRING` | メモ・説明 |
| `--due=STRING` | 期日（RFC3339 または `YYYY-MM-DD`） |
| `--parent=STRING` | 親タスク ID（サブタスクとして作成） |
| `--previous=STRING` | 前のタスク ID（順序指定） |
| `--repeat=STRING` | 繰り返し: `daily`, `weekly`, `monthly`, `yearly` |
| `--repeat-count=INT` | 繰り返し回数（`--repeat` 必須） |
| `--repeat-until=STRING` | 繰り返し終了日（`--repeat` 必須） |

```bash
# 基本追加
gog tasks add <tasklistId> --title "買い物"

# 期日付き
gog tasks add <tasklistId> --title "レポート提出" --due "2025-01-20"

# メモ付き
gog tasks add <tasklistId> --title "会議準備" --notes "資料を印刷する"

# サブタスク
gog tasks add <tasklistId> --title "サブタスク" --parent <parentTaskId>

# 毎週繰り返し（4回）
gog tasks add <tasklistId> --title "週報" --due "2025-01-06" --repeat weekly --repeat-count 4

# 毎月末まで繰り返し
gog tasks add <tasklistId> --title "月次チェック" --due "2025-01-01" --repeat monthly --repeat-until "2025-12-31"
```

## タスク更新

```bash
gog tasks update <tasklistId> <taskId> [flags]
```

`add` と同じフラグが使用可能。

```bash
# タイトル変更
gog tasks update <tasklistId> <taskId> --title "新しいタイトル"

# 期日変更
gog tasks update <tasklistId> <taskId> --due "2025-02-01"

# メモ追加
gog tasks update <tasklistId> <taskId> --notes "追加メモ"
```

## タスク完了

```bash
gog tasks done <tasklistId> <taskId>
```

エイリアス: `complete`

## タスク未完了に戻す

```bash
gog tasks undo <tasklistId> <taskId>
```

エイリアス: `uncomplete`, `undone`

## タスク削除

```bash
gog tasks delete <tasklistId> <taskId>
```

エイリアス: `rm`, `del`

## 完了済みタスクのクリア

```bash
gog tasks clear <tasklistId>
```

タスクリスト内の完了済みタスクをすべて削除する。
