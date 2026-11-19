---
name: mutation-can-run-real-side-effects
description: 守りを外す変異の下では、試験が止まらずに本物の docker・台の予約を動かす。試験で subprocess を塞ぎ、回し手が回の前後で副作用を数える
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-19T06:57:33.869Z
---

変異の試験は、守り(引数の照らし・入口の止め)を外した写しで試験を回す。守りの手前で止まる前提の試験は、変異の下では止まらずに先へ進み、本物の命令を呼ぶ。

実例(2026-11-19 14:52・dkb): EC2 手元の演習の組み立ての部品で「D-KG なしの組み立てを止める」行を外した変異の下、試験 S13 の `up(w)` が本物の Docker と rehearse_env の予約で最後まで走った。変異は「捕えた」と数えられ、一時の置き場は消えたが、容器 3・網・置き場・台の予約が LOCKS の無いまま約 1 時間残った。気づいたのは次の演習の前の確かめ(待ち受けと容器の数)。

**Why:** 部品の既定の `run=subprocess.run` は定義の時に結ばれるので、試験で `subprocess.run` を差し替えても既定には効かない。試験が偽の run を渡す所だけ守られ、渡さない所(up の中の台の部品)は本物を呼ぶ。

**How to apply:**
- 本物の外部(docker・台・DB・EC2)を呼ぶ部品の試験では、setUp で `subprocess.run` と入口の守りを塞ぐ。部品の `run` の既定は None にして、呼ぶ時に `subprocess.run` を引く。
- 変異の回し手は、各回の前後で副作用(演習の容器・網・置き場・予約のファイル)を数え、増えたら止める。始める前に 0 であることも照らす。
- 演習や台の前には、待ち受けと容器を数えて、自分の知らない物が無いことを確かめる(今回はこれで見つけた)。

関連 [[test-child-must-block-llm]] [[mutation-harness-needs-control]] [[rig-acceptance-must-check-writes-outside]]
