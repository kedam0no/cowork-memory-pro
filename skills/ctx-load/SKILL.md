---
name: ctx-load
description: Load only task-relevant sections from memory, filtered by keyword analysis
---

# Context Load (Selective)

Load the minimum necessary context from past sessions.

## When to invoke

- After ctx-analyze has produced keywords
- At session start / after compact

## Algorithm (atomic format)

1. Read `~/.cowork-memory/memory.md`
2. Identify project sections (`## project-name`)
3. Score each section against ctx-analyze keywords:
   - **+3** — section name matches project keyword exactly
   - **+2** — section contains task_type or tech keyword
   - **+1** — section contains any keyword
4. Always include `## _global` and `## _tasks` (incomplete items only)
5. Load sections with score ≥ threshold (from settings.json `relevance_threshold`)
6. Cap at `max_sessions_loaded` sections

## Algorithm (legacy session-block format)

Same scoring as before — split on `---`, score each block, load top N.

## Relevance thresholds

- `"high"` — score ≥ 4
- `"medium"` — score ≥ 2 (default)
- `"low"` — score ≥ 1

## Output

```
[Smart Context: loaded projects: <list> + _tasks]
```

If nothing matched:
```
[Smart Context: no relevant history found]
```

## Fallback

If keywords are empty (new session), load `## _global`, `## _tasks`, and the 2 most recently updated project sections.
