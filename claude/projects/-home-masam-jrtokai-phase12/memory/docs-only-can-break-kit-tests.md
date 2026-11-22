---
name: docs-only-can-break-kit-tests
description: docs の下だけの差でも、道具の試験が docs/review_samples/ の束を全部照らすので、新しい欄の見本を道具より先に本体へ入れると本体の試験が落ちる
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-11-21T17:10:15.132Z
---

「文書だけの差」でも、本体の試験を壊し得る。

2026-11-22 01:05(1dbe84e07)に、fed が review_kit 改訂 15 の合成の見本(docs/review_samples/fedkit/・改訂 15 の新しい欄を持つ束)を、agy の点検のために「文書だけ」として本体に入れた。改訂 15 の道具は、まだ枝にしか無かった。

その結果、次の 2 件の試験が本体で落ちた。
- `test_rk_hidden_marks.py::test_git_synthetic_sets_all_zero`
- `test_rk_r14a_check.py::test_git_synthetic_sets_still_pass`

この 2 件は、git の管理下の docs/review_samples/**/*.set.json を全部、本体の道具の check で照らす。fed-reload の試験の全部を回した時に気づいた。

**Why:** 「docs の外 = 0」の確かめ(main-ff-docs-only-during-rig の規律)は、道具のコードを変えない事を確かめるだけである。道具の試験が docs の下の何を読むかまでは見ていない。

**How to apply:** 本体へ docs の差を入れる前に、次を行う。
- 道具の試験が glob で読む置き場(docs/review_samples/ など)に触れていないかを確かめる。
- 触れていれば、本体の版の道具で、その試験を写しの上で回してから入れる。
- 新しい欄を持つ見本は、道具と同じ便で本体に入れる。
- 点検のためだけに先に置く時は、道具の試験が読まない置き場を選ぶ。

関連: [[main-ff-docs-only-during-rig]] / [[verify-what-the-target-actually-reads]] / [[check-what-the-target-mechanism-scans]]
