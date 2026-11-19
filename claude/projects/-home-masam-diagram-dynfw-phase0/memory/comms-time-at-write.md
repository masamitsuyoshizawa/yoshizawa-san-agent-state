---
name: comms-time-at-write
description: "連絡文の名と date 行の時刻は、書く直前の `date` の分をそのまま使う。次の分を見越して付けない"
metadata:
  node_type: memory
  type: feedback
  originSessionId: fdc4c948-da33-4847-b9a6-04212d37500d
  modified: 2026-10-28T11:29:43.650Z
---

連絡文(docs/comms/)の名の `HHMM` と `date:` 行は、Write の直前に取った `date` の分をそのまま使う。照らしの記録の中で先に「req HHMM」と書く場合も、書いた後に実の時刻と照らし、違えば直す。

**Why:** 2026-10-28 に 2 回、書く時刻より 1 分先の名(19:58・20:29)を付けて、未来の時刻を書かない決まりに反した。1 回目はコミット前に直し、2 回目はコミット・push の後に名を直す追加のコミットになった。

**How to apply:** 連絡文を書く手順は「`date` → その分で名と date 行を書く → コミット前に `date` で照らす」。2 つの連絡文を同じ分に書くのは構わない。関連: [[short-paste-texts]]
