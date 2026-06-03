---
name: mem-extract
description: Extract atomic facts from the current conversation — decisions, discoveries, preferences, tasks
---

# Memory Extract

Scan the conversation and pull out only what's worth remembering. No user input required.

## When to invoke

- After completing a task or making a decision
- When the topic shifts significantly
- Before mem-merge (always paired)
- At session end as part of auto-save

## What counts as a fact

Extract when you see:

| Type | Example |
|------|---------|
| 決定事項 | 「Rustで書くことに決めた」「APIはPOSTにする」 |
| 発見・学習 | 「mem0はV3でシングルパス抽出に変えた」 |
| ユーザーの好み | 「シンプルなファイルベースを好む」「日本語でやりとり」 |
| プロジェクト事実 | 「ブランチ名: voice-sample-eval-nZUs1」 |
| 未完了タスク | 「音声サンプルの評価実装がまだ」 |

## What NOT to extract

- 雑談・あいさつ
- `<private>...</private>` の中身
- すでに明らかに既知の事実（抽象的すぎるもの）
- 一時的な確認（「はい」「わかりました」）

## Output format

Return a structured list — internal use only, not shown to user:

```json
{
  "project": "cowork-memory-pro",
  "facts": [
    {
      "type": "decision",
      "text": "mem0のV3抽出アルゴリズムをcowork-memory-proに実装する",
      "project": "cowork-memory-pro"
    },
    {
      "type": "task_incomplete",
      "text": "voice-sample-eval: callpceとcrushon.ai音声サンプルの評価実装",
      "project": "voice-sample-eval"
    },
    {
      "type": "preference",
      "text": "インフラ不要・ファイルベースのシンプルさを優先",
      "project": null
    }
  ]
}
```

## Fact types

- `decision` — 決定したこと
- `discovery` — 調査・学習で判明したこと
- `preference` — ユーザーの好み・スタイル
- `project_fact` — プロジェクト固有の事実（パス、ブランチ、設定など）
- `task_incomplete` — 未完了タスク
- `task_done` — 完了したタスク（既存の incomplete を消すために使う）
