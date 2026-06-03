---
name: mem-sync
description: Subagent-side skill — push findings to shared memory safely with lock coordination
---

# Memory Sync (Subagent)

Subagent専用。自分の発見・成果をメイン記憶に安全に書き込む。
メインエージェント（くろちゃん）はこのスキルを使わない — mem-saveを使う。

## When to invoke

- サブエージェントがタスクを完了したとき
- 重要な発見をしたとき（メインに報告する前に記録）
- サブエージェントのセッション終了前

## Steps

1. **自分のagent-idを決める** — 引数で渡されるか、タスク内容から生成
   例: `sub:research`, `sub:code-review`, `sub:db-audit`
2. **mem-extract** を実行して発見事実を抽出
3. **lock取得** → mem-merge（agent-id付き）→ **lock解放**

## Lock protocol

```
lockファイル: ~/.cowork-memory/memory.lock
```

**取得**:
- lockファイルが存在しない → 作成して内容に `agent-id: <id>\ntimestamp: <unix>` を書く
- lockファイルが存在する → 1秒待って再試行、最大10回
- lockが30秒以上古い（stale） → 強制上書き（前のエージェントがクラッシュしたとみなす）

**解放**:
- 書き込み完了後、lockファイルを削除

## Agent-id付きfact形式

mem-mergeに渡すfactにagent-idを追加する:

```json
{
  "agent_id": "sub:research",
  "facts": [
    {
      "type": "discovery",
      "text": "mem0のV3はシングルパス抽出でLoCoMo 91.6点を達成",
      "project": "cowork-memory-pro"
    }
  ]
}
```

## Output

```
[Sync: sub:research → 3 facts written]
```

エラー時:
```
[Sync failed: lock timeout after 10s — retry or check for stale lock]
```
