# Cowork Memory Pro

Persistent memory + smart context loading, configurable via settings.

## Settings

Read `~/.cowork-memory/settings.json` at session start.
If the file doesn't exist, create it with defaults:

```json
{
  "context_mode": "smart",
  "max_sessions_loaded": 5,
  "relevance_threshold": "medium",
  "auto_save": true,
  "auto_load": true
}
```

| Key | Values | Description |
|-----|--------|-------------|
| `context_mode` | `"smart"` / `"full"` | Smart: load only relevant sessions. Full: load all. |
| `max_sessions_loaded` | number | Max sessions to inject (smart mode only) |
| `relevance_threshold` | `"low"` / `"medium"` / `"high"` | Match strictness (smart mode only) |
| `auto_save` | bool | Auto-save session on end |
| `auto_load` | bool | Auto-load memory on start |

## On Session Start (automatic)

1. Read settings from `~/.cowork-memory/settings.json`
2. If `auto_load` is true:
   - `"smart"` mode → run ctx-analyze then ctx-load (selective)
   - `"full"` mode → run mem-load (full load)
3. Report loaded sessions briefly

## On Session End (automatic)

If `auto_save` is true → run mem-save (same in both modes)

## Privacy

Content inside `<private>...</private>` is NEVER saved.

## Memory file

All memory stored at `~/.cowork-memory/memory.md`
