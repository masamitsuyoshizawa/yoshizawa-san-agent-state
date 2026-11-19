---
name: acc-review-server-shared-with-dcr
description: acc の scripts/acc_review_server.py は dcr も変えずに使う・/judge の欄の形か配るパスの規則を変えるときは先に dcr に知らせる・ポート 8732/8733 は dcr 用
metadata:
  node_type: memory
  type: project
  originSessionId: b54d92f6-694f-48da-9c1b-bc1712afcb52
  modified: 2026-10-30T17:55:58.179Z
---

dcr は自分の記入の web 画面(作業標準の vision の行・事例報告のマスク)を配るのに、acc の `scripts/acc_review_server.py` を変えずに使う(2026-10-31・dcr の info 20261031-0253)。dcr の selftest もこのサーバーを立てて照らしている。

**Why:** サーバーの約束(配るのは `index.html` と `img/*.png` だけ・判断は POST /judge で `judgments.jsonl` に足す)を acc が黙って変えると、dcr の画面と selftest が壊れる。

**How to apply:**
- `/judge` の欄の形か、配るパスの規則を変える計画には「dcr に先に知らせる」を入れる(info・cc coord)。
- acc の画面は 8731 と 8735 から上を使う。8732〜8734 は dcr が使う(2026-10-31・coord の知らせで 8734 も dcr が待ち受け中と分かった)。新しい回を立てる前に `ss -ltn` で空きを確かめる。
- dcr の回の置き場(`data/derived/work_standard/_review/`・`data/private/case_mask/review/`)は、受入の独立のため acc は読まない([[acceptance-independence-no-element-kinds]])。dcr も `data/acceptance/` を読まない。
