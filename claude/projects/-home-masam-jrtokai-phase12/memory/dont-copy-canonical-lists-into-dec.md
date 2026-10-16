---
name: dont-copy-canonical-lists-into-dec
description: 他者所有の一覧(理由コード等)を dec に写すと版ずれで 3 回訂正する羽目になる(2026-10-16 決262 追補 2〜5)
metadata:
  type: feedback
---

coord が ckb 所有の理由コード一覧を「相当」の例として dec に書いた(見ずに書いた)→ ckb がその例に合わせて設計書を改訂 → coord は改訂 0 の 5 つを正本と書く → fed/dkb が別々の版に合わせる、という行き違いが 1 時間で 3 回起きた(決262 追補 2・3・4・5)。

**Why:** 名を dec に写した瞬間に、所有者の文書と dec の 2 つの正本ができ、どちらかが動くとずれる。
**How to apply:** 他者所有の一覧・名・件数は dec に写さず「正本 = <文書の現行版の節>」と指す。例を書くなら現物を grep してから書き、「相当」「等」で濁さない。所有者が改訂中なら、改訂の完了を待ってから参照する。関連 [[verify-filenames-before-citing]] [[assumed-current-state-without-checking]]。
