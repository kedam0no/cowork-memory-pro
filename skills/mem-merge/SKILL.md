---
name: mem-merge
description: Merge extracted facts into memory.md using ADD/UPDATE/DELETE/NOOP logic — multi-agent safe
---

# Memory Merge

Apply extracted facts to `~/.cowork-memory/memory.md` with smart deduplication.
Multi-agent safe: always called while holding the lock (from mem-save or mem-sync).

## When to invoke

- From mem-save (main agent) — after acquiring lock
- From mem-sync (subagent) — after acquiring lock
- Never invoked directly

## Memory file format

```markdown
# Cowork Memory

## [project-name]
- [YYYY-MM-DD][main] fact text here
- [YYYY-MM-DD][sub:research] fact found by subagent

## _global
- [YYYY-MM-DD][main] ユーザーの好み・横断的な事実

## _tasks
- [ ] incomplete task description (YYYY-MM-DD)
- [x] completed task description (YYYY-MM-DD)
```

`[agent-id]` タグ: `main`（メイン）または `sub:<name>`（サブエージェント）。
agent-idがない場合は `[main]` とみなす（後方互換）。

## Merge algorithm

For each fact from mem-extract / mem-sync:

1. **Find candidates** — scan memory for facts in the same project section that are semantically similar
2. **Decide operation**:

| Condition | Operation |
|-----------|-----------|
| No similar fact exists | **ADD** — append with `[YYYY-MM-DD][agent-id]` |
| Similar fact exists, same content | **NOOP** — skip |
| Similar fact exists, content updated | **UPDATE** — replace line (preserve original agent-id) |
| Fact explicitly contradicts existing | **DELETE** — remove old line |
| `task_done` received | **DELETE** matching `- [ ]` in `_tasks` |

3. **Apply** — write updated file atomically (write to tmp → rename)

## Section rules

- `type: decision / discovery / project_fact` → `## [project]` section
- `type: preference` → `## _global` section
- `type: task_incomplete` → `## _tasks` as `- [ ] ...`
- `type: task_done` → mark matching task as `- [x] ...`

## Atomic write

Always write via temp file to prevent corruption on crash:
1. Write new content to `~/.cowork-memory/memory.tmp`
2. Rename to `memory.md` (atomic on most filesystems)

## File bootstrap

If `~/.cowork-memory/memory.md` doesn't exist:
- Create directory
- Write header `# Cowork Memory`
- Apply ADD operations

## Migration from old format

If file contains old session-block format (`--- session: ...`):
- Move old blocks to `## _archive` section
- Write new atomic facts in proper sections

## Output

Silent — caller (mem-save or mem-sync) reports completion.
