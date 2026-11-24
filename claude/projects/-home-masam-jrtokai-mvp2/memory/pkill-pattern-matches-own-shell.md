---
name: pkill-pattern-matches-own-shell
description: pkill -f の型を、同じ Bash の呼び出しの字に書くと、その呼び出しのシェルも止まる(終了 144)・ポートの番号で止める(2026-11-24・dgr)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-24T06:59:29.230Z
---

2026-11-24、配りの止めに `pkill -f 'p1_s1_cam_add.py serve --sid … --port 8756'` を、LOCKS の書き換えとコミットと同じ Bash の呼び出しに並べた。型が呼び出しのシェルのコマンドの字にも当たり、シェルごと止まって(終了 144)、後の LOCKS の解放とコミットが走らなかった。

**Why:** `pkill -f` は、全部のプロセスのコマンドの字を照らす。呼び出しのシェルの字にも、同じ字が入っている。
**How to apply:**
- 配りの止めは、ポートを聞いているプロセスの番号で止める(`ss -ltnp` か `fuser -k 8756/tcp`)。または `pkill -f` を単独の呼び出しにし、型の 1 字を `[p]1_s1…` の形にして、自分に当たらないようにする。
- 止めた後は、別の呼び出しで、ポートが閉じたか・LOCKS とコミットが済んだかを確かめる。
- 関連: [[no-exit-in-bash-guards]]
