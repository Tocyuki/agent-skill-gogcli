# Classroom コマンドリファレンス

Google Classroom API。**教育機関（Google Workspace for Education）向け**。

## コース操作

### コース一覧

```bash
gog classroom courses list [flags]
```

### コース詳細

```bash
gog classroom courses get <courseId>
```

### コース作成

```bash
gog classroom courses create --name "コース名" [flags]
```

### コース更新

```bash
gog classroom courses update <courseId> [flags]
```

### コース削除

```bash
gog classroom courses delete <courseId>
```

### コースのアーカイブ / 復元

```bash
gog classroom courses archive <courseId>
gog classroom courses unarchive <courseId>
```

### コースへの参加 / 離脱

```bash
gog classroom courses join <courseId>
gog classroom courses leave <courseId>
```

### コースの Web URL

```bash
gog classroom courses url <courseId> ...
```

## 名簿（Roster）

コースの生徒・教師の一覧を一括取得:

```bash
gog classroom roster <courseId> [flags]
```

### 生徒操作

```bash
gog classroom students <command>
```

### 教師操作

```bash
gog classroom teachers <command>
```

## 課題（Coursework）

エイリアス: `work`

### 課題一覧

```bash
gog classroom coursework list <courseId> [flags]
```

### 課題詳細

```bash
gog classroom coursework get <courseId> <courseworkId>
```

### 課題作成

```bash
gog classroom coursework create --title "課題名" <courseId> [flags]
```

### 課題更新

```bash
gog classroom coursework update <courseId> <courseworkId> [flags]
```

### 課題削除

```bash
gog classroom coursework delete <courseId> <courseworkId>
```

### 課題の割り当て対象変更

```bash
gog classroom coursework assignees <courseId> <courseworkId> [flags]
```

## 教材（Materials）

```bash
gog classroom materials <command>
```

## 提出物（Submissions）

### 提出物一覧

```bash
gog classroom submissions list <courseId> <courseworkId> [flags]
```

### 提出物詳細

```bash
gog classroom submissions get <courseId> <courseworkId> <submissionId>
```

### 提出（Turn in）

```bash
gog classroom submissions turn-in <courseId> <courseworkId> <submissionId>
```

### 取り下げ（Reclaim）

```bash
gog classroom submissions reclaim <courseId> <courseworkId> <submissionId>
```

### 返却（Return）

```bash
gog classroom submissions return <courseId> <courseworkId> <submissionId>
```

### 成績評価（Grade）

```bash
gog classroom submissions grade <courseId> <courseworkId> <submissionId> [flags]
```

## お知らせ（Announcements）

```bash
gog classroom announcements <command>
```

## トピック（Topics）

```bash
gog classroom topics <command>
```

## 招待（Invitations）

```bash
gog classroom invitations <command>
```

## 保護者（Guardians）

```bash
gog classroom guardians <command>
gog classroom guardian-invitations <command>
```

## ユーザープロフィール

```bash
gog classroom profile <command>
```
