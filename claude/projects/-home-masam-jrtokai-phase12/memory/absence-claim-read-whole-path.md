---
name: absence-claim-read-whole-path
description: 「処理が無い」と書く前に、値の生成から応答までの経路を全部たどる(freq_* の LOO 差し引きを見落とし、利用者の決定 決244 (3) を誤らせた・2026-10-14)
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-14T13:14:17.341Z
---

M01 の集計表に「現行の dkg は freq_* を LOO で数え直していない」と書いた。根拠にしたのは、点数の式(dkg_backend.py 134 行)だけだった。実際には後段に `_apply_loo_exclusion`(178 行・2026-08-26 導入)があり、exclude_ids を渡すと必ず差し引く。この記述を前提に、利用者が是正を決めてしまった(決244 (3))。是正の依頼を読んで実装の場所を探したときに気づき、取り下げられた(追補 1)。読むだけで測ると、残るずれは引きすぎ 98/647 問で、候補の上位 5 は全問で同じだった(決245 で「是正しない」)。

**Why:** 「無い」という主張は、経路の一部を読んだだけでは成り立たない。しかも利用者の判断の前提に直接なる。

**How to apply:** 「〜していない」「〜が無い」と書く前に、対象の値(例: freq の fc/fp/fi)を grep で全部たどる。読むのは、値を作る所・書き換える所・応答に出す所。書き換える関数があれば、それがどこから呼ばれるかも確かめる。[[assumed-current-state-without-checking]] [[claim-wider-than-implementation]] [[verify-zero-counts-before-reporting]]
