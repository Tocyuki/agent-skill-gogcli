# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Claude Code plugin that provides a `gog` skill for operating Google Workspace services (Gmail, Calendar, Drive, Sheets, Tasks, Docs, Slides, Contacts, People, Chat, Classroom, Groups, Keep) via the [gogcli](https://github.com/steipete/gogcli) CLI tool. This is a content-only plugin (no source code to build/test) — all files are Markdown and JSON.

## Architecture

**Single-skill, progressive disclosure design:**

- `skills/gog/SKILL.md` — Main skill entry point and routing layer. Contains frontmatter (name, description, allowed-tools), prerequisite checks, basic principles, and quick references for each service. Must stay under 400 lines.
- `skills/gog/reference/*.md` — Detailed per-service command references. SKILL.md links to these for full syntax/flags/examples.
- `.claude-plugin/plugin.json` — Plugin manifest (name, version, author, keywords).
- `.claude-plugin/marketplace.json` — Marketplace distribution manifest.

**Key design decisions:**
- All Google Workspace services are unified under one skill (`gog`) rather than split per-service, because cross-service workflows are common.
- SKILL.md acts as a router: it shows 3-5 representative commands per service, then links to the reference file for full details.
- The skill's `description` field contains Japanese trigger phrases ("メールを確認", "予定を作成", etc.) to enable natural language auto-triggering.
- `allowed-tools` restricts execution to `Bash(gog *)`, `Bash(which gog)`, `Bash(brew install steipete/tap/gogcli)`, and `Read`.

## Validation

```bash
# Local plugin test
claude --plugin-dir /path/to/agent-skill-gogcli

# Verify gogcli is available
gog --version

# Check command accuracy against actual CLI help
gog <service> --help
gog <service> <command> --help
```

## Content Guidelines

- Explanations and headings: Japanese
- Command syntax, flag names, API terms: English as-is
- Technical terms (OAuth, API, JSON, CLI, scope): English as-is
- All command examples must match actual `gog --help` output — verify before adding new commands
- Reference files target 100-350 lines each
