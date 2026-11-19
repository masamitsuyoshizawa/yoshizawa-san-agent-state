---
name: phase-marker-state-must-be-unique
description: 「段を過ぎた」を判じる印には、その段でしか出ない値を使う(処理役の件の状態 running は回 1〜3 でも出る)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-10T05:18:35.527Z
---

時点や段を「過ぎた」と判じる条件には、その段に入ったときだけ現れる印(例 工程の状態の `g3_apply_started`)を使う。複数の段で出る状態の値(例 処理役の件の状態 `running`)を印にしない。否定例を書くときも、その値が出るほかの段(回 1〜3)の場面を作って、誤って当たらないことを確かめる。

**Why:** 2026-11-10 の決352 追補 23 の Q-A の 1 回目で、止める係の S2 が「件 b が running なら過ぎた」としていたため、回 1 の間に子 2 の行を誤って「外した」と記録した。dkb の照らしは外した行が 1 つでもあれば例を不合格に数えるので、台を down して回し直した。試験の否定例も running を過ぎたの例にしていて、仕様の誤りを写していた。

**How to apply:** 時点の判じ方を書くときは、使う値ごとに「どの段で出るか」を一覧にし、目的の段だけで出る物を印にする。試験には、その値がほかの段で出る場面を否定例として入れる。[[bulk-intake-r352-bkb]] [[check-both-branches-of-a-condition]]
