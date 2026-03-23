---
name: mem-reflect
description: Generate a summary or review of recent work patterns from memory
---

# Memory Reflect

Use this skill to generate insights from accumulated session history.

## When to invoke

- When the user asks "最近何やってた?", "今月の進捗は?", etc.
- When the user uses `/mem-reflect`
- Weekly review requests

## What to generate

Analyze `~/.cowork-memory/memory.md` and produce:

1. **最近の作業** — Last 7 days of activity (bullet summary)
2. **繰り返しのテーマ** — Topics/projects that appear frequently
3. **未完了タスク** — All open items across all sessions
4. **注目ポイント** — Anything that was deferred multiple times (potential blocker)

## Output format

```
[Memory Reflect — YYYY-MM-DD 時点]

### 最近の作業 (7日間)
- YYYY-MM-DD: <summary>
- YYYY-MM-DD: <summary>

### 未完了タスク (全セッション)
- [ ] <task> (最終: YYYY-MM-DD)

### 繰り返しテーマ
- <theme>: N回

### 注目
- <deferred/blocked items>
```
