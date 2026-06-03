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

## Multi-Agent (くろちゃん + ローカル部隊)

メインエージェント（くろちゃん）とサブエージェント（ローカル部隊）は同じ `memory.md` を共有する。

**役割分担**:
- **Main (くろちゃん)**: `mem-save` を使う。タスク分解・指示・統合担当
- **Sub (ローカル部隊)**: `mem-sync` を使う。調査・実装・発見を記憶に書き込む

**書き込み競合の解決**: lockファイル方式
```
~/.cowork-memory/memory.lock  ← 書き込み中のエージェントが保持
~/.cowork-memory/memory.tmp   ← アトミック書き込み用
```

書き込みフロー:
1. lock取得（最大10回リトライ、30秒以上古いlockはstaleとみなし強制取得）
2. mem-mergeでfact書き込み（tmpファイル経由でアトミックにrename）
3. lock解放

**メモリ上でのエージェント識別**:
```markdown
- [YYYY-MM-DD][main] くろちゃんが記録した事実
- [YYYY-MM-DD][sub:research] ローカル部隊が発見した事実
```

## Memory format

`~/.cowork-memory/memory.md` stores atomic facts, not session transcripts:

```markdown
# Cowork Memory

## project-name
- [YYYY-MM-DD][main] fact about this project
- [YYYY-MM-DD][sub:research] fact found by subagent

## _global
- [YYYY-MM-DD][main] user preference or cross-project fact

## _tasks
- [ ] incomplete task (YYYY-MM-DD)
- [x] completed task (YYYY-MM-DD)
```

Facts are deduplicated via ADD/UPDATE/DELETE/NOOP logic on every save.

## Privacy

Content inside `<private>...</private>` is NEVER extracted or saved.

## Memory file

All memory stored at `~/.cowork-memory/memory.md`
