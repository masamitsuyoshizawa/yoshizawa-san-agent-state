---
name: b-canonical-conn-container-env
description: 手元の B 正本(neo4j-accident-v7・9890)への接続は、利用者の決め(2026-11-18・案 A)でコンテナの起動設定 NEO4J_AUTH から取り、コマンドの中でだけ使う。~/.kb_ec2.env は EC2 のトンネル(19890・commit)なので使わない
metadata:
  node_type: memory
  type: reference
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-17T15:58:00.583Z
---

手元の B 正本へ dkb の会話からつなぐ方法(利用者の決め 2026-11-18 00 時台・会話・案 A「コンテナの起動設定 (推奨)」を選んだ)。

- Bash ツールのシェルは毎回ログインの設定から始まり、`KB_ACCIDENT_NEO4J_*` は入っていない(2026-11-17 に数えて 0 個)。
- 包み: 作業用の置き場に `b_env_run.sh` を作り、`docker inspect -f '{{range .Config.Env}}{{println .}}{{end}}' neo4j-accident-v7` の `NEO4J_AUTH=user/pw` を割って `KB_ACCIDENT_NEO4J_URI=bolt://localhost:9890`・`_USER`・`_PASSWORD` に入れ、`KB_CONN_MODE` を外して `exec "$@"`。値は表示しない・ファイルに残さない。スクラッチパッドは再起動で消えるので、消えていたら作り直す。
- `~/.kb_ec2.env` は B の変数を定める唯一のファイルだが、接続先 `localhost:19890`・`KB_CONN_MODE=commit`(EC2 へのトンネルと推定)。手元の正本の読みには使わない。
- 固定版の作業の枝(dkb-r351w)の道具は、`DKB_MAIN_TREE=/home/masam/jrtokai-phase12` が無いと位置の表を作業の枝の木の中に探して止まる。
- 2026-11-17 に、取り方を作業メモから探す操作は権限の判定で止められた。利用者の決めの前に探し直さない。
- 使う時は毎回 LOCKS の行と info・前後の graph_digest(export_graph_apoc)。

**Why:** 利用者が方法を決めるまで 10 時間以上、束が止まった(info 20261117-1351)。
**How to apply:** B を読む作業の前に、この包みで digest を取ってつながりを確かめる。関連 [[r351-review-kit-set-dkb]] [[route-questions-to-coord]]
