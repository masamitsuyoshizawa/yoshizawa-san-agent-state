---
name: crossing-proposals-both-adapt
description: 双方が同時に相手の案へ合わせると逆に食い違う(決266 の口の引数・2026-10-19)・合わせる前に相手の最新版を現物で見る
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-19T07:55:01.276Z
---

fed が dkb の q_table 案(dkb 改訂 4)に合わせて改訂 6 を出したのと同じ時刻に、dkb は fed の元の案(q_numbers_json)で改訂 5 を確定し q_table を取り下げていた。両方が相手に合わせた結果、逆向きに食い違った(fed 改訂 7 で dkb 改訂 5 に揃えて解消)。

**Why:** 連絡は会話で先に届き、相手の文書はその後も動く。届いた案に即座に合わせると、相手も同時にこちらへ合わせていた場合に入れ替わる。

**How to apply:** 相手の対案を受けて自分の文書を合わせる前に、main を取り込み相手の設計の最新版(sha16 と改訂番号)を現物で確かめる。合わせたら「どの版に合わせたか」を sha16 つきで相手に返す。行き違ったら、相手の確定版(後に「確定」と書いた方)に揃え、自分の古い rep に撤回の注記を付ける。関連 [[relay-positions-verify-at-ask-time]]・[[verify-filenames-before-citing]]
