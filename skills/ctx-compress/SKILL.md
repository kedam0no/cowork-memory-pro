---
name: ctx-compress
description: Compress and trim the current context window by removing low-relevance content
---

# Context Compress

Reduce context size by dropping irrelevant loaded sections before hitting limits.

## When to invoke

- When `/ctx-compress` is called
- When context window is detected to be large
- Proactively suggest when context feels heavy

## How to compress

1. Review what's currently loaded in context
2. Re-score each loaded session block against the **current** task keywords
3. Drop blocks that are no longer relevant (score dropped to 0 due to task shift)
4. Summarize dropped blocks into a 1-line archive entry
5. Keep: current task sessions + last 2 sessions regardless of score

## Output

```
[Context compressed: removed N sessions, kept M]
Current topic: "<detected task>"
```

## Important

- Never drop 未完了タスク (incomplete task) entries from any session
- Never drop the most recent session
- If unsure whether to drop something, keep it
