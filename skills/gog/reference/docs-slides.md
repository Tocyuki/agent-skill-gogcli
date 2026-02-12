# Docs / Slides コマンドリファレンス

## Google Docs

### ドキュメントをプレーンテキストで表示

```bash
gog docs cat <docId> [flags]
```

ドキュメントの内容をプレーンテキストとして出力。

```bash
gog docs cat <docId>
```

### ドキュメント情報

```bash
gog docs info <docId>
```

ドキュメントのメタデータ（タイトル、リビジョン等）を表示。

### ドキュメントのエクスポート

```bash
gog docs export <docId> [flags]
```

対応フォーマット: `pdf`, `docx`, `txt`

```bash
# PDF にエクスポート
gog docs export <docId> --format pdf

# DOCX にエクスポート
gog docs export <docId> --format docx

# テキストにエクスポート
gog docs export <docId> --format txt
```

### ドキュメントの作成

```bash
gog docs create <title> [flags]
```

```bash
gog docs create "新しいドキュメント"
```

### ドキュメントのコピー

```bash
gog docs copy <docId> <title> [flags]
```

```bash
gog docs copy <docId> "コピー - レポート"
```

## Google Slides

### プレゼンテーション情報

```bash
gog slides info <presentationId>
```

スライド数、タイトル等のメタデータを表示。

### プレゼンテーションのエクスポート

```bash
gog slides export <presentationId> [flags]
```

対応フォーマット: `pdf`, `pptx`

```bash
# PDF にエクスポート
gog slides export <presentationId> --format pdf

# PPTX にエクスポート
gog slides export <presentationId> --format pptx
```

### プレゼンテーションの作成

```bash
gog slides create <title> [flags]
```

```bash
gog slides create "Q1 プレゼンテーション"
```

### プレゼンテーションのコピー

```bash
gog slides copy <presentationId> <title> [flags]
```

```bash
gog slides copy <presentationId> "テンプレートコピー"
```

## 共通パターン

### ドキュメント / スライドの ID 取得

Drive 検索で ID を取得してからエクスポートするワークフロー:

```bash
# Drive で検索
gog drive search "プレゼン資料" --json

# 結果から fileId を使ってエクスポート
gog docs export <fileId> --format pdf
gog slides export <fileId> --format pdf
```

### ドキュメントの内容確認 → エクスポート

```bash
# まず内容を確認
gog docs cat <docId>

# 必要に応じてエクスポート
gog docs export <docId> --format pdf
```
