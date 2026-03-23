---
name: mem-save
description: Save current session summary to persistent memory
---

# Memory Save

Use this skill to save the current session's context for future recall.

## When to invoke

- Automatically when the session ends
- When the user says "保存して", "覚えておいて", "記録して", etc.
- When the user uses `/mem-save`

## What to save

Capture:
- **作業内容**: What was actually done (concise bullets)
- **決定事項**: Key decisions and their rationale
- **未完了タスク**: Work that was started but not finished
- **重要なコンテキスト**: File paths, credentials structure, usernames, project-specific facts, user preferences

## What NOT to save

- Content inside `<private>...</private>` tags
- Sensitive credentials or tokens (note their existence/location, not the value)
- Verbose conversation transcript
- Trivial exchanges

## Save format

Append to `~/.cowork-memory/memory.md`:

```markdown

---
session: YYYY-MM-DD HH:MM
project: <project name>
---

## 作業内容
- item

## 決定事項
- item

## 未完了タスク
- item

## 重要なコンテキスト
- item
```

## File management

- Create `~/.cowork-memory/` directory if it doesn't exist
- Append new sessions; do not overwrite old ones
- If file exceeds 50KB: summarize entries older than 30 days into a `## アーカイブ` section at the top
