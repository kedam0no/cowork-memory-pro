---
name: mem-search
description: Search past session memory for specific topics, decisions, or context
---

# Memory Search

Use this skill to search for specific information across all past sessions.

## When to invoke

- When the user asks about something from a previous session
- When you need to verify a past decision before proceeding
- When the user uses `/mem-search <query>`

## How to search

1. Read `~/.cowork-memory/memory.md`
2. Scan all session entries for entries matching the query
3. Return matching snippets with session date context

## Output format

```
[Memory search: "<query>"]

Found in session YYYY-MM-DD:
> <relevant snippet>

Found in session YYYY-MM-DD:
> <relevant snippet>
```

If nothing found: `[Memory: "<query>" に関する記録なし]`

## Query types

- **Project name**: Return all sessions for that project
- **Task/keyword**: Return sessions mentioning that topic
- **Date range**: Return sessions from that period (e.g., "先週", "今月")
- **Decision**: Search 決定事項 sections specifically
