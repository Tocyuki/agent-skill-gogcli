# Sheets コマンドリファレンス

## セル値の取得

```bash
gog sheets get <spreadsheetId> <range> [flags]
```

| フラグ | 説明 |
|--------|------|
| `--dimension=STRING` | 主軸: `ROWS`（デフォルト）, `COLUMNS` |
| `--render=STRING` | 値のレンダリング: `FORMATTED_VALUE`, `UNFORMATTED_VALUE`, `FORMULA` |

```bash
# 範囲指定で取得
gog sheets get <spreadsheetId> "Sheet1!A1:D10"

# シート全体
gog sheets get <spreadsheetId> "Sheet1"

# 数式を取得
gog sheets get <spreadsheetId> "Sheet1!A1:D10" --render FORMULA

# 列方向で取得
gog sheets get <spreadsheetId> "Sheet1!A1:D10" --dimension COLUMNS

# JSON で取得（加工用）
gog sheets get <spreadsheetId> "Sheet1!A1:D10" --json
```

### 範囲の指定方法

- `Sheet1!A1:D10` — シート名 + セル範囲
- `Sheet1!A:D` — 列全体
- `Sheet1!1:10` — 行全体
- `Sheet1` — シート全体
- `A1:D10` — 最初のシートのセル範囲

## セル値の更新

```bash
gog sheets update <spreadsheetId> <range> [<values> ...] [flags]
```

| フラグ | 説明 |
|--------|------|
| `--input="USER_ENTERED"` | 入力方式: `RAW`（そのまま）, `USER_ENTERED`（数式・日付解釈） |
| `--values-json=STRING` | JSON 2D 配列で値を指定 |
| `--copy-validation-from=STRING` | データバリデーションをコピー元範囲から適用 |

```bash
# 単一セル更新
gog sheets update <spreadsheetId> "Sheet1!A1" "新しい値"

# 複数セル（1行）
gog sheets update <spreadsheetId> "Sheet1!A1:C1" "val1" "val2" "val3"

# JSON で更新（複数行）
gog sheets update <spreadsheetId> "Sheet1!A1:C2" --values-json '[["a","b","c"],["d","e","f"]]'

# 数式を挿入
gog sheets update <spreadsheetId> "Sheet1!A1" "=SUM(B1:B10)"

# RAW 入力（数式を文字列として扱う）
gog sheets update <spreadsheetId> "Sheet1!A1" "=SUM" --input RAW
```

## 行の追加（Append）

```bash
gog sheets append <spreadsheetId> <range> [<values> ...] [flags]
```

指定範囲の末尾に新しい行を追加する。

```bash
# 1行追加
gog sheets append <spreadsheetId> "Sheet1!A:D" "val1" "val2" "val3" "val4"

# JSON で複数行追加
gog sheets append <spreadsheetId> "Sheet1!A:D" --values-json '[["a","b","c","d"],["e","f","g","h"]]'
```

## セルのクリア

```bash
gog sheets clear <spreadsheetId> <range>
```

```bash
gog sheets clear <spreadsheetId> "Sheet1!A1:D10"
```

## セルのフォーマット

```bash
gog sheets format <spreadsheetId> <range> [flags]
```

セルの書式設定を適用する。

## スプレッドシートのメタデータ

```bash
gog sheets metadata <spreadsheetId>
```

シート名、シート数、プロパティ等の情報を取得。

## 新規スプレッドシート作成

```bash
gog sheets create <title> [flags]
```

```bash
gog sheets create "売上レポート 2025"
```

## スプレッドシートのコピー

```bash
gog sheets copy <spreadsheetId> <title>
```

## エクスポート

```bash
gog sheets export <spreadsheetId> [flags]
```

Drive API 経由で PDF / XLSX / CSV にエクスポート。

```bash
# PDF エクスポート
gog sheets export <spreadsheetId> --format pdf

# XLSX エクスポート
gog sheets export <spreadsheetId> --format xlsx

# CSV エクスポート
gog sheets export <spreadsheetId> --format csv
```
