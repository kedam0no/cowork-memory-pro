# Cowork Memory Pro

Persistent memory + smart context loading. Fully automatic — no user input required.

## Settings

Read `~/.cowork-memory/settings.json` at session start.
If the file doesn't exist, create it with defaults:

```json
{
  "context_mode": "smart",
  "max_sections_loaded": 5,
  "relevance_threshold": "medium",
  "auto_save": true,
  "auto_load": true
}
```

| Key | Values | Description |
|-----|--------|-------------|
| `context_mode` | `"smart"` / `"full"` | Smart: load only relevant sections. Full: load all. |
| `max_sections_loaded` | number | Max project sections to inject (smart mode only) |
| `relevance_threshold` | `"low"` / `"medium"` / `"high"` | Match strictness (smart mode only) |
| `auto_save` | bool | Automatically extract and save facts during and after session |
| `auto_load` | bool | Auto-load memory on start |

## On Session Start (automatic)

1. Read settings from `~/.cowork-memory/settings.json`
2. If `auto_load` is true:
   - `"smart"` mode → run ctx-analyze then ctx-load (selective)
   - `"full"` mode → run mem-load (full load)
3. Report briefly: open tasks + loaded projects

## During Session (automatic)

If `auto_save` is true, silently run **mem-save** (extract → merge) when:

- A task is completed
- A significant decision is made
- The topic shifts to a new project
- Something important is discovered or learned

Do this silently — no report to user unless they ask.
This means memory stays current throughout the session, not just at the end.

## On Session End (automatic)

If `auto_save` is true → run mem-save one final time to capture anything remaining.
Report: `[Memory saved: N facts updated]`

## Memory format

`~/.cowork-memory/memory.md` stores atomic facts, not session transcripts:

```markdown
# Cowork Memory

## project-name
- [YYYY-MM-DD] fact about this project
- [YYYY-MM-DD] another fact

## _global
- [YYYY-MM-DD] user preference or cross-project fact

## _tasks
- [ ] incomplete task (YYYY-MM-DD)
- [x] completed task (YYYY-MM-DD)
```

Facts are deduplicated via ADD/UPDATE/DELETE/NOOP logic on every save.

## Privacy

Content inside `<private>...</private>` is NEVER extracted or saved.

## Memory file

All memory stored at `~/.cowork-memory/memory.md`
