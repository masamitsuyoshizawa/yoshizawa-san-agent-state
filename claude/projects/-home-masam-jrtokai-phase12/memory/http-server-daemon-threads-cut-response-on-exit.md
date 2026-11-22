---
name: http-server-daemon-threads-cut-response-on-exit
description: ThreadingHTTPServer(daemon_threads)は、保存の錠を待っても、返事の本文を書く前にプロセスが終わり得る。止めの試験が時々落ちたら疑う
metadata:
  type: feedback
---

`http.server.ThreadingHTTPServer` は `daemon_threads = True` なので、処理の糸はプロセスの終わりで黙って止まる。止めで「保存の錠が取れるまで待つ」だけでは、保存は記録されても、その後に書く返事の本文が切れる(画面の側では `IncompleteRead` か `RemoteDisconnected`)。

2026-11-22、review_kit (b) の 7 回目で起きた。保存の途中で止める試験(T20)が、手元で 10 回に 1〜5 回落ちた。前の版の写しでは 10 回とも通ったので、時の運で隠れていた(推定: 前から在った)。止めで処理中の要求の数を数え、返事を書き終えるまで待つ(上限 2 秒)形に直した。返事の本文を書く前に子で遅らせる差し替え(send_slow)を入れると、必ず再現した。

**Why:** 時々しか落ちない試験を「揺れ」として再試行で流すと、本物の取りこぼしを見逃す。

**How to apply:** 止めの道の試験が時々落ちたら、落ちた時の例外を取る(pytest の `-rw` で糸の例外が出る)。窓が狭い競合は、差し替えで窓を広げた試験を作り、必ず落ちる事を前の版で確かめてから直す。

関連: [[mutation-harness-needs-control]] / [[negative-test-check-reason]]
