---
name: review-kit-answers-kit-commit-is-head
description: review_kit の答えの頭の行の kit_commit は、serve を立てた時の本体ツリー全体の HEAD(git rev-parse --short=9 HEAD)で、部品を最後に変えたコミットではない
metadata:
  type: reference
---

`scripts/review_kit/cli.py` の `kit_commit()` は `git -C <review_kit> rev-parse --short=9 HEAD` = 本体ツリー全体の HEAD を返す。serve・read が答えの頭の行(review-answers/1)などに書く kit_commit はこの値。agy の点検の版(部品を最後に変えたコミット・例 887ca7f49)とは違う値になるのが普通。

**Why:** 2026-11-18 の第 2 弾 実の回 1 の serve で、頭の行の値を読む前に「kit_commit 887ca7f49」と知らせに書き、実物は 8d108b31c(直前の LOCKS のコミット)だった。訂正した。
**How to apply:** 配る時の記録では「部品の最後の変更のコミット(git log -1 -- scripts/review_kit)」と「頭の行の kit_commit(HEAD)」を分けて書き、2 つの間の部品の差 0 を `git diff <点検の版> <kit_commit> -- scripts/review_kit` で示す。知らせの数や値は、出力を読んでから書く。関連: [[tested-commit-must-be-run-commit]]
