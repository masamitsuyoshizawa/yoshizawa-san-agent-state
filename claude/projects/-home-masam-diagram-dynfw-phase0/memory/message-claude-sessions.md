---
name: message-claude-sessions
description: Claude Code のセッション(icc)への処理開始・回答を読む依頼は、貼る文でなく SendMessage で送る(2026-10-31 利用者の指示)
metadata:
  node_type: memory
  type: feedback
  originSessionId: fdc4c948-da33-4847-b9a6-04212d37500d
  modified: 2026-10-31T14:27:00.890Z
---

Claude Code のセッション(icc。ListAgents の名は `diagram-icc`)に、新しい依頼の処理を始めさせるときや、回答を読ませるときは、利用者に貼る文を渡さず、coord が SendMessage で送る。Codex のセッション(icx・rev)と Antigravity(iag)には SendMessage は届かない。2026-11-21 からは ai-hub が自動で知らせる([[ai-hub-auto-notify]])。ai-hub が止まっている時だけ、貼る文を示す([[short-paste-texts]])。

**Why:** 利用者の時間の節約(2026-10-31 の指示「今後 claude code のセッションには時間節約の為、処理開始や回答を読む依頼は、メッセージングを使用するようにしてください」)。

**How to apply:** req をコミットして INBOX を作り直した後に、`SendMessage(to: "diagram-icc", message: "INBOX.md の新しい依頼を処理してください" …)` を送る。終わりを知りたいときは `notify_when_idle: true` を付ける。送る前に ListAgents で名を確かめる(名が変わる・セッションが無いときは貼る文に戻す)。最後の報告では「icc には送った」と書く。icx・iag・rev は、ai-hub の打ち込みを `ai-hub log` で確かめて書く。
