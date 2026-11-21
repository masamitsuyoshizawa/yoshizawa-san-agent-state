---
name: rehearsal-local-path-skips-prod-transport
description: 演習を「local」の口だけで回すと、当日の運び(ssh 越し)を 1 度も通さない — 同じ道具・同じ版でも、運ぶ道が違えば演習は当日の証しにならない
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-21T04:22:05.660Z
---

決351 の EC2 の演習(10 回 × 3 巡・全部合格)は、50 問の道具を `--ssh local`(シェルを通さない)で回していた。当日の W-1(2026-11-21)で `--ssh <EC2>` にした途端、`_cmd` が引数を並べて ssh に渡すため、向こうのシェルで `--format "{{.Id}} {{.State.StartedAt}} {{.State.Running}}"` が 3 つに割れ、手順 1 で止まった(coord が見つけた)。試験の偽の run も「引数の並びに語が在るか」で答えを分け、ssh の 1 本の字の形を見られない作りだった。

**Why:** 「演習と当日で同じ道具・同じ版」を担保にしていたが、運ぶ道(local と ssh)が違い、当日の道は試験でも演習でも通っていなかった。

**How to apply:**
- 当日に使う口の形(ssh・トンネル・別の名)ごとに、演習か試験で 1 度は通す。通せない時は、本番と同じ作りの偽(ssh なら「残りを空白でつないで sh -c に解かせる」)で、local と同じ答えになる事を試験にする。
- 偽の物が甘くない事を、直す前の版で今日の実物の失敗を再現できるかで確かめる(変異の期待の形に入れる)。
- 「同じ版」と書く時は、運ぶ道も同じかを併せて書く。関連 [[test-fixture-must-match-prod-setup]] [[synthetic-fixture-hides-rewrite-defects]] [[relay-positions-verify-at-ask-time]]
