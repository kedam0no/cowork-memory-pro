---
name: mem-save
description: Save current session to persistent memory via automatic extraction and deduplication
---

# Memory Save

Orchestrates mem-extract → mem-merge. No session block appending — only atomic facts.

## When to invoke

- Automatically at session end (if `auto_save: true`)
- Automatically after a significant task completes or decision is made
- When the user says "保存して", "覚えておいて", "記録して"
- When the user uses `/mem-save`

## Steps

1. Run **mem-extract** on the current conversation
2. If no facts were extracted → skip (nothing to save)
3. Run **mem-merge** with the extracted facts
4. Report result briefly

## Output

```
[Memory saved: N facts updated]
```

If nothing new: `[Memory: no new facts to save]`

## Important

- Never append raw conversation text
- Never create session blocks
- Content inside `<private>...</private>` is never extracted or saved
- Sensitive values (tokens, passwords) are noted by location only, never by value
