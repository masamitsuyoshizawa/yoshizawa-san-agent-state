---
name: tmp-neo4j-password-leading-dash
description: 一時 Neo4j コンテナのパスワードを secrets.token_urlsafe で作ると先頭が「-」のとき(約 1.5%)neo4j-admin set-initial-password が旗と読んで起動しない。token_hex にする(2026-09-27 dkb が実測・coord 承認 rep 2034)
metadata:
  type: feedback
---

dkb が D-KG の順の試作中に、戻した D-KG の一時コンテナが起動せず(ログ「Missing required parameter: '<password>'」)、原因は `secrets.token_urlsafe(12)` の先頭が「-」だったこと。10 万回の試行で 0.0154 の割合。パイプで失敗が隠れて両方を正本で測ってしまい、破棄して取り直した。

**Why:** 起動の引数に渡す秘密の文字列は、シェルや CLI が旗と読む文字(先頭「-」)を含まない生成器で作る。失敗はパイプの終了コードで隠れる(merge-failure-masked-by-pipe と同型)。

**How to apply:** 一時コンテナのパスワードは `secrets.token_hex` で作る。dkb の 3 本(rehearse_env.py・rehearse_vocab_scenarios.py・vocab_dump.py)は承認済み・ckb の rehearse_env_c.py は ckb の判断。「戻した DB」を使う測定は、両側の接続先(db.info の名前とポート)を出力に残して正本と区別する。[[merge-failure-masked-by-pipe]] [[claim-wider-than-implementation]]

**直し済み(2026-09-27 20:35 dkb・main 5b9a8b42)**: rehearse_env.py 98d000d2・rehearse_vocab_scenarios.py a6f75fe6・vocab_dump.py b096131f(+7 −7・test_tmp_password 3 件)。ckb の rehearse_env_c.py は ckb 待ち。

**ckb 分も直し済み(2026-09-28 09:02・affe2d11)**: rehearse_env_c.py 5e17b77e99778e7d(cypher-shell -p の別引数に渡す型も同じ穴・試験 45・演習 rc 0)。C 正本 9990 = 台帳 c(95ddf4a6c5047b5b・8,055/16,585)一致を再確認。
