---
name: notify-before-advancing-frozen-version
description: 「もう変えない」と書いた版を進める時は、進める前に coord に知らせる(rev は自動で即座に読むので、出した後の差し替えは行き違いになる)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-14T05:15:12.203Z
---

「もう変えない」と固定を宣言した版(設計書・道具)を、相手の念押しなどで直したくなったら、**直す前に coord に知らせて了解を取ってから**進める。直してから組を差し替えるのは不可。

**Why:** coord は固定の宣言を受けるとすぐ rev に依頼を出し、rev は自動で即座に読み始める。後から版を進めると依頼と行き違い、coord が依頼に追記し rev の作業ツリーにも届け直す手間が出る。2026-11-14 に bkb が第 2 弾の設計を改訂 5 で固定と書いた直後、fed の念押し(作り手の段の入力は review-read/1 だけ)で改訂 6 に進め、rev への依頼(20261114-1410)と行き違った。coord から「これで 3 回目」と指摘された(決351 第 2 弾の設計)。

**How to apply:** 固定を宣言する前に、相手(fed・dkb)の最新の連絡と念押しを全部読んでから宣言する。宣言の後に直す必要が出たら、まず coord に「何を・なぜ直したいか」を送り、直すかどうかの返事を待つ。[[locks-for-follow-up-changes]] [[relay-positions-verify-at-ask-time]]
