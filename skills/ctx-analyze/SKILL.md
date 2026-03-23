---
name: ctx-analyze
description: Analyze the current conversation to extract task keywords for selective context loading
---

# Context Analyze

Extract what the user is currently working on to guide selective memory loading.

## When to invoke

- At session start before loading any memory
- After `/compact`
- Before any ctx-load operation

## How to analyze

Scan the most recent user messages and identify:

1. **Project name** — repo name, app name, or domain (e.g., "brain.py", "cowork-memory")
2. **Task type** — what kind of work (e.g., "bug fix", "新機能", "設定")
3. **Key entities** — file names, function names, service names mentioned
4. **Language/tech** — Python, TypeScript, Docker, etc.

## Output

Return a structured keyword set (internal use, not shown to user):

```
{
  "project": "cowork-memory",
  "task_type": "plugin development",
  "keywords": ["plugin", "memory", "CLAUDE.md", "skills"],
  "tech": ["markdown", "json"]
}
```

## Fallback

If the session has no prior messages (brand new session), return empty keywords.
ctx-load will then use only recency (most recent N sessions) as fallback.
