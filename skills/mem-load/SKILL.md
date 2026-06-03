---
name: mem-load
description: Load persistent memory — supports both atomic fact format and legacy session-block format
---

# Memory Load

Load context from `~/.cowork-memory/memory.md`.

## When to invoke

- Automatically at the start of every session
- When the user asks "前回どこまでやったっけ?" or similar
- When the user references past work you don't have context for

## How to load

1. Read `~/.cowork-memory/memory.md`
2. Detect format:
   - **Atomic format** (has `## _tasks` or project sections with `- [YYYY-MM-DD]` lines) → parse as facts
   - **Legacy format** (has `--- session:` blocks) → parse as session blocks
3. Inject into context

## Parsing atomic format

- Each `## project-name` section = facts for that project
- `## _global` = user preferences and cross-project facts
- `## _tasks` = all tasks; surface only `- [ ]` (incomplete) ones

## Output

```
[Memory loaded]
未完了タスク: <list of open tasks, if any>
関連プロジェクト: <list of project sections found>
```

Keep it concise — do not dump the entire file.
