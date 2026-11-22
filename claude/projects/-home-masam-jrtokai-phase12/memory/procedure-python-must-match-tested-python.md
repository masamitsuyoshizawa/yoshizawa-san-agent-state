---
name: procedure-python-must-match-tested-python
description: 手順書・別紙の命令の python は、試験と演習で使った物と同じ絶対の道で書き、DB に届く前に import を確かめる行を置く
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-22T09:19:57.676Z
---

手順書や別紙に人が打つ命令を書く時は、python を `python3` と書かず、試験と演習で使った物と同じ絶対の道(本 PJ = `/home/masam/jrtokai-phase12/venv/bin/python3`・neo4j 6.1.0)で書く。最初の行に、その python で要る部品(`import neo4j`)が通る事を確かめる行を置く。

**Why:** 2026-11-22 の決351 の今夜の前の確かめ(EC2 を読むだけ)で、別紙の `T="python3 -B …"` がシステムの python3 を指していた。そこに neo4j のドライバが無く、static が UNEXPECTED(ModuleNotFoundError)で止まった。試験 88 本と手元の演習は全部 venv の python で回していたので、試験では見えなかった。前倒しで当てたので見つかったが、当日なら窓の中で止まる所だった(coord が指摘)。

**How to apply:** 別紙を書いたら、命令の python と、試験の実行の記録の python を並べて照らす。道具の中の子の python(PY 定数)とも同じ物にする。関連: [[test-must-not-inject-what-prod-lacks]] / [[rehearsal-local-path-skips-prod-transport]]
