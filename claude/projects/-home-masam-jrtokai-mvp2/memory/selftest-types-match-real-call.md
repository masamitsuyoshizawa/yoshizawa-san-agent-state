---
name: selftest-types-match-real-call
description: 合成の試験は実の呼び出しと同じ型で渡す — selftest が Counter を渡し、実の build は dict を返していて、実の書き出しの 1 枚目で KeyError(2026-11-04・dgr・b62ea32)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-04T07:34:57.105Z
---

関数を足して selftest で試すときは、引数を実の呼び出しの道筋と同じ型で作る(例: `build` の戻りの `dict(cnt)` をそのまま渡す。試しの側で `Counter()` を新しく作らない)。

**Why:** dgr の `kgb_export.apply_decisions` は selftest 28/28 で通ったが、実の 3 枚の書き出しの 1 枚目で `KeyError` で止まった(selftest は Counter、実は dict)。書く前に止まったので被害は無かったが、コミットと書き出しをやり直した。

**How to apply:** 渡す物を書き出す前に、実のデータでメモリの上だけで回す(write を差し替えて書かない)。selftest の入力は実の関数の戻りから作る。[[commit-tool-before-writing-handoff]] の「試しはメモリの上で」と同じ流れ。

再発(2026-11-24・dgr・p1_s3_parts close): 合成の単位は機械の候補(cands)を持ち、実の閉じた K の単位は持たない。「このとおり」の答えを閉じる所で KeyError。書く前にメモリの上で実の段 2 を回して見つけた(書く前だったので害は無い)。合成の単位を作る時は、実の置き場の鍵の並びと照らす。
