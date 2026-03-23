---
name: mem-load
description: Load persistent memory from previous Cowork sessions
---

# Memory Load

Use this skill to load context from previous sessions.

## When to invoke

- Automatically at the start of every session
- When the user asks "前回どこまでやったっけ?" or similar
- When the user references past work you don't have context for

## How to load

1. Read `~/.cowork-memory/memory.md`
2. Parse all session entries
3. Identify the most recent and most relevant entries for the current task
4. Inject that context into your understanding of the project

## Output

After loading, briefly summarize what you found:

```
[Memory loaded]
前回 (YYYY-MM-DD): <1 sentence summary>
未完了タスク: <list if any>
```

Do this concisely — do not dump the entire memory file to the user.
