---
name: mutation-restore-stale-pyc
description: 変異を戻してもソースの sha 一致では足りない — 同じ大きさの変異を同じ秒の中で戻すと __pycache__ の変異版が使われ続ける
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-16T14:38:54.046Z
---

変異の試験で、ソースを `cp` で戻して sha の一致を確かめたのに、後の試験が変異の版の振る舞いで落ちた(2026-10-16 23:30・ingest_bundle.py の照らす順の入れ替え・N1b)。原因: 入れ替えはファイルの大きさを変えず、変異の書き込み(23:30:18.37 に pyc 生成)と戻し(23:30:18.39)が同じ秒。pyc の検証は秒単位の mtime と大きさなので、古い pyc が有効と判定された。

**Why:** ソースの sha 一致は「ファイルが戻った」の証拠で、「次に読まれる版が戻った」の証拠ではない。その間に回した試験は変異の版で通っていたかもしれない。

**How to apply:** 変異の試験は `PYTHONDONTWRITEBYTECODE=1` で回すか、戻した後に `__pycache__/<名>.*.pyc` を消し、正例の試験を回して合格を確かめてから先へ進む。報告には「戻した後の正例の合格」を併記する。関連: [[negative-test-check-reason]]
