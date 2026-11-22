---
name: impl-added-codes-go-into-design-before-freeze
description: 実装の途中で設計に無い札やふるまいを足したら、「もう変えない」の前に設計の表に足す(rep に書くだけでは足りない)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-11-21T21:01:20.041Z
---

実装の途中で、設計の表に無い札やふるまいを足した時は、「もう変えない」を出す前に、設計の表にもその行を足す。rep に書くだけでは足りない。

2026-11-22、review_kit (b) の後片付けの実装(fed-cleanup 15892f02b)で起きた。書き直しが名に出た後に置き場の同期だけが失敗した時について、G2(値が残るかもしれない時は必ず知らせる)に照らして、札 `SECRET_SYNC` を実装で足した。rep には書いたが、設計 6 回目(sha16 ef33dbae37ebf324)の表には足さないまま固めた。coord が設計の本文を検索して、名が無い事を見つけた。rev は固めた版を見るので、その回は枝も設計も動かせず、次の版へ回った。

**Why:** rev と coord は「設計の表の行」と「実装」を突き合わせて照らす。表に無い札は、照らしの外になるか、表と実装の食い違いとして出る。どちらにしても往復が 1 回増える。

**How to apply:** 固める前に、実装で出す札の名を、全部 grep で設計の本文と突き合わせる。表に無い名が在れば、表に行を足してから固める。新しい判断(G2 などの決まりに照らした物)は、表の行に理由も書く。

関連: [[spec-unverified-until-implemented]] / [[claim-wider-than-implementation]]
