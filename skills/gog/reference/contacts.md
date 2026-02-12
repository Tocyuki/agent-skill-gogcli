# Contacts / People コマンドリファレンス

## Google Contacts

### 連絡先検索

```bash
gog contacts search <query> ... [flags]
```

名前、メールアドレス、電話番号で検索。

```bash
# 名前で検索
gog contacts search "田中"

# メールアドレスで検索
gog contacts search "tanaka@example.com"

# JSON で取得
gog contacts search "田中" --json
```

### 連絡先一覧

```bash
gog contacts list [flags]
```

```bash
gog contacts list
gog contacts list --json
```

### 連絡先詳細

```bash
gog contacts get <resourceName>
```

### 連絡先作成

```bash
gog contacts create [flags]
```

```bash
gog contacts create --name "山田太郎" --email "yamada@example.com" --phone "090-1234-5678"
```

### 連絡先更新

```bash
gog contacts update <resourceName> [flags]
```

### 連絡先削除

```bash
gog contacts delete <resourceName>
```

### Workspace ディレクトリ

Workspace 組織のディレクトリ連絡先:

```bash
gog contacts directory <command>
```

### その他の連絡先

自動的に追加された連絡先（メール送信先等）:

```bash
gog contacts other <command>
```

## Google People

Workspace ディレクトリのユーザー情報にアクセスする。

### 自分のプロフィール

```bash
gog people me
```

### ユーザープロフィール取得

```bash
gog people get <userId>
```

### ディレクトリ検索

```bash
gog people search <query> ... [flags]
```

Workspace ディレクトリ全体を検索。

```bash
# 名前で検索
gog people search "yamada"

# JSON で取得
gog people search "tanaka" --json
```

### ユーザーの関係性

```bash
gog people relations [<userId>] [flags]
```

組織図上の関係（マネージャー等）を表示。
