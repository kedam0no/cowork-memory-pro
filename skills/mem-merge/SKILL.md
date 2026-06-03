---
name: mem-merge
description: Merge extracted facts into memory.md using ADD/UPDATE/DELETE/NOOP logic
---

# Memory Merge

Apply extracted facts to `~/.cowork-memory/memory.md` with smart deduplication.
Never appends blindly — always compares against existing facts first.

## When to invoke

- Always after mem-extract (as a pair)
- Never invoked alone

## Memory file format

`memory.md` uses atomic fact format grouped by project:

```markdown
# Cowork Memory

## [project-name]
- [YYYY-MM-DD] fact text here
- [YYYY-MM-DD] another fact

## [another-project]
- [YYYY-MM-DD] fact text here

## _global
- [YYYY-MM-DD] ユーザーの好み・横断的な事実

## _tasks
- [ ] incomplete task description (YYYY-MM-DD)
- [x] completed task description (YYYY-MM-DD)
```

## Merge algorithm

For each fact from mem-extract:

1. **Find candidates** — scan memory for facts in the same project section that are semantically similar
2. **Decide operation**:

| Condition | Operation |
|-----------|-----------|
| No similar fact exists | **ADD** — append to project section |
| Similar fact exists, content is the same | **NOOP** — skip |
| Similar fact exists, content has changed or is updated | **UPDATE** — replace the old line |
| Fact explicitly contradicts or invalidates existing fact | **DELETE** — remove old line |
| `task_done` type received | **DELETE** matching `_tasks` incomplete entry |

3. **Apply** — write the updated file

## Section rules

- `type: decision / discovery / project_fact` → goes into `## [project]` section
- `type: preference` → goes into `## _global` section
- `type: task_incomplete` → appended to `## _tasks` as `- [ ] ...`
- `type: task_done` → find matching `- [ ]` in `_tasks` and mark as `- [x] ...`

## Creating new sections

If a project section doesn't exist yet, create it:

```markdown
## new-project-name
- [YYYY-MM-DD] first fact
```

## File bootstrap

If `~/.cowork-memory/memory.md` doesn't exist:
- Create `~/.cowork-memory/` directory
- Write the file with just the header `# Cowork Memory`
- Then apply ADD operations

## Migration from old format

If the file contains old session-block format (`--- session: ...`):
- Leave old blocks untouched in an `## _archive` section
- Start writing new atomic facts below existing content
- Do not try to convert old blocks automatically

## Output

Silent — no output to user. The calling skill (mem-save) reports completion.
