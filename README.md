# agent-skill-gogcli

Google Workspace CLI ([gogcli](https://github.com/steipete/gogcli)) skill plugin for Claude Code.

Gmail, Calendar, Drive, Sheets, Tasks, Docs, Slides, Contacts, Chat, Classroom, Groups, Keep を自然言語で操作できます。

---

## Installation / インストール

### Prerequisites / 前提条件

Install gogcli first / 先に gogcli をインストール:

```bash
brew install steipete/tap/gogcli
```

Then authenticate / 認証:

```bash
gog auth credentials ~/Downloads/client_secret_xxx.json
gog auth add you@gmail.com
```

### Install Plugin / プラグインインストール

```bash
claude plugin add Tocyuki/agent-skill-gogcli
```

Or for local development / ローカル開発:

```bash
claude --plugin-dir /path/to/agent-skill-gogcli
```

## Features / 対応サービス

| Service | Commands | Description |
|---------|----------|-------------|
| **Gmail** | search, get, send, labels, drafts, thread, batch | メール検索・送信・ラベル管理 |
| **Calendar** | events, create, update, delete, freebusy, conflicts | 予定管理・空き時間確認 |
| **Drive** | ls, search, upload, download, share, mkdir | ファイル管理・共有 |
| **Sheets** | get, update, append, clear, format, create | スプレッドシート読み書き |
| **Tasks** | lists, list, add, done, undo, delete | タスク管理 |
| **Docs** | cat, export, create, copy | ドキュメント操作 |
| **Slides** | export, info, create, copy | スライド操作 |
| **Contacts** | search, list, create, update, delete | 連絡先管理 |
| **People** | me, get, search, relations | プロフィール参照 |
| **Chat** | spaces, messages, dm | メッセージ送信 (Workspace) |
| **Classroom** | courses, roster, coursework, submissions | 教育管理 |
| **Groups** | list, members | グループ管理 (Workspace) |
| **Keep** | list, get, search | メモ管理 (Workspace) |

## Usage / 使い方

Natural language / 自然言語で操作:

```
"未読メールを確認して"
"明日の予定を教えて"
"ドライブで議事録を検索して"
"スプレッドシートのA1:D10を取得して"
"タスクリストに「買い物」を追加して"
```

Direct invocation / 直接呼び出し:

```
/gogcli:gog Gmailの未読を検索
/gogcli:gog 今週のカレンダーを表示
```

## Plugin Structure / プラグイン構成

```
agent-skill-gogcli/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest
│   └── marketplace.json     # Marketplace manifest
├── skills/
│   └── gog/
│       ├── SKILL.md         # Main skill (routing layer)
│       └── reference/
│           ├── setup.md     # Auth & setup guide
│           ├── gmail.md     # Gmail reference
│           ├── calendar.md  # Calendar reference
│           ├── drive.md     # Drive reference
│           ├── sheets.md    # Sheets reference
│           ├── tasks.md     # Tasks reference
│           ├── docs-slides.md  # Docs + Slides reference
│           ├── contacts.md  # Contacts + People reference
│           ├── chat.md      # Chat reference (Workspace)
│           ├── classroom.md # Classroom reference
│           ├── advanced.md  # Groups, Keep, Service Account
│           └── workflows.md # Cross-service workflows
├── README.md
└── LICENSE
```

## License

MIT
