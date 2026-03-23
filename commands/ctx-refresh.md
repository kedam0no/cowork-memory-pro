---
name: ctx-refresh
description: Re-analyze the current task and reload only relevant context
---

現在の会話内容を再分析して、関連するコンテキストだけを再読み込みしてください。

1. ctx-analyze で現在のタスクキーワードを抽出
2. ctx-load でキーワードに一致するセッションのみ読み込み
3. 読み込んだセッション数とトピックを報告
