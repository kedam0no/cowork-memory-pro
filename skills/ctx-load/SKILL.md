---
name: ctx-load
description: Load only task-relevant sections from memory, filtered by keyword analysis
---

# Context Load (Selective)

Load the minimum necessary context from past sessions.

## When to invoke

- After ctx-analyze has produced keywords
- At session start / after compact

## Algorithm

1. Read `~/.cowork-memory/memory.md`
2. Split into individual session blocks (separated by `---`)
3. For each session block, score relevance:
   - **+3** — project name matches exactly
   - **+2** — task_type or tech matches
   - **+1** — any keyword found in the block
   - **+1** — session is within last 7 days (recency bonus)
4. Sort by score descending
5. Load top N sessions (default: 5, configurable via settings.json `max_sessions_loaded`)
6. Discard blocks with score 0 (completely unrelated)

## Relevance thresholds (from settings.json)

- `"high"` — only load score ≥ 4
- `"medium"` — load score ≥ 2 (default)
- `"low"` — load score ≥ 1

## Output

Inject matched sessions into context silently, then report:

```
[Smart Context: X sessions loaded (topic: "<project>")]
```

If no sessions matched:
```
[Smart Context: no relevant history found]
```

## Fallback

If keywords are empty (new session), load the 3 most recent sessions regardless of score.
