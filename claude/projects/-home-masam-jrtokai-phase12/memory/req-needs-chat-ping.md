---
name: req-needs-chat-ping
description: 連絡箱に req を置いたら、同じ応答の中で、相手のセッションに会話(SendMessage)でも一言知らせる
metadata:
  node_type: memory
  type: feedback
  originSessionId: 0934ae6b-71b5-4dab-9416-391e81b1b2ba
  modified: 2026-11-23T18:21:08.834Z
---

連絡箱(`docs/comms/`)に req を置いたら、**同じ応答の中で、相手のセッションに、会話(SendMessage)でも、便の名と要点を知らせる**。後の便で中身が置き換わった時は、「どれが今も生きているか」を書く。

**Why:** 2026-11-24 に、rkb が bkb と dkb へ出した req 各 5 通が、3〜4 時間、気づかれなかった(coord の指摘)。相手は、自分の枝の `INBOX.md` と連絡箱だけでは気づかない事が在る(rkb の枝を、相手が取り込むまで、相手の木に便が無い。見張りの自動の知らせは、coord が `INBOX.md` を作った時だけ)。

**How to apply:** req・足しの req を出す手番で、`ListAgents` の名(例 `jrtokai-phase12-bkb`)へ送る。1 行目に、何の便かを書く。事故の記録の字は、会話にも書かない。関連 [[session-comms-worktree]]・[[route-questions-to-coord]]。
