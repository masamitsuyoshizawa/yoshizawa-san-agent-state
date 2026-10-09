---
name: equality-checks-need-a-timepoint
description: 検査を「A == B == C」と時点抜きで書くと、正常な運用を不合格にする
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-09-23T23:14:16.495Z
---

**等式の検査は「いつの値どうしか」を持たせる。** 時点を書かずに `A == B == C` と
並べると、健全な状態を落とす。

実例(2026-09-24・V3 第 2 段): `graph_digest == meta.vocab_graph_digest == registry`
の三者一致と書いた。新しい版 R2 が登録された後、まだ R1 を持っている読み手は
registry(最新 = R2)と一致しない。正常なのに不合格になる。
正しくは「既存の束 = 読込時に確認した値と照合」「新規読込 = そのときの最新値と照合」。

**Why:** 「固定して持つ」と設計側が決めていても、検査側が時点のない等式で書くと
その設計が消える。値が違うのが正常な場面を、検査が知らないまま落とす。

**How to apply:** 複数の値の一致を書くときは、それぞれが「いつ取った値か」を先に言う。
場面(読込済み / 新規)で照合先が変わるなら、検査も場面で分ける。
「最新と一致しないから不合格」と書く前に、最新と一致しないのが正常な場面が
在るかを数える。[[claim-wider-than-implementation]]
