---
name: mem-sync
description: Subagent — sync findings to shared memory — usage: /mem-sync <agent-id>
---

mem-sync スキルを実行してください。agent-id は "$ARGUMENTS"。

このエージェントの発見・成果を共有メモリに同期します：
1. lockを取得
2. mem-extractで発見を抽出
3. mem-mergeでagent-id付きで書き込み
4. lockを解放

完了後に `[Sync: <agent-id> → N facts written]` を報告してください。
