---
name: coord-merges-peer-branch-when-peer-main-push-blocked
description: 利用者の決め(2026-11-19): 担当の側で本体(main)への取り込みや push が自動の許可の判定で止められた時は、coord が中身を確かめた上で取り込んでよい(その都度、報告する)
metadata:
  type: feedback
---

担当(bkb など)が「main への早送りと push が、自動の許可の判定で止められた。coord の merge で入れてほしい」と言ってきた時は、coord が、その枝を取り込んで main へ進めてよい。

**Why:** 2026-11-19、bkb の rep に上の一文が在った。相手のセッションで止められた操作を coord が代わりに行うのは、その判定のすり抜け(permission laundering)に当たるので、coord は取り込まずに利用者に諮った。利用者の答え =「coord が取り込む(今後も)」(決351 追補 129 の 1)。

**How to apply:** 取り込む前に、枝と main の差を見て、(1) ファイルの種類(連絡・設計・道具・試験・架空の見本など)(2) `accident_kb_v7/data/`・`knowledge_kb_v8/data/`・補完辞書・鍵の類が 0、を確かめる。取り込んだら、利用者への報告と、dec の追補に、枝のコミットとファイルの数を書く。この承認は「本体への取り込みと push」に限る。ほかの種類の、止められた操作(DB への書き込み・EC2 の操作・設定の変更など)を、代わりに行う承認ではない。それらは、今までどおり、行わずに利用者に諮る。関連 [[session-comms-worktree]]


(索引から移した説明・2026-11-20)
- [担当の main への取り込みが止められた時は coord が取り込む](coord-merges-peer-branch-when-peer-main-push-blocked.md) — 利用者の決め 2026-11-19・中身を確かめて・報告する・取り込みと push に限る
