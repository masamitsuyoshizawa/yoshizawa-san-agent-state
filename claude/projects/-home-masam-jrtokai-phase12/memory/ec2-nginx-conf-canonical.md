---
name: ec2-nginx-conf-canonical
description: 旧 EC2 の nginx(jrtokai-demo-nginx)の動いている設定はコンテナの中だけ(sha16 bf11960ce6750eb3)・ディスクの deploy/nginx-new/nginx.conf は 9/4 の古い版・コンテナを作り直さない(2026-11-04 coord)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-04T11:55:09.030Z
---

EC2のnginx(jrtokai-demo-nginx)はconfをイメージに焼き込む方式(reload不可・build→up必須)。
2026-09-04の事故: EC2上のリポジトリコピーのconfが稼働版より古く(difyブロックなし)、
そのまま再ビルドして**difyが到達不能になった**。保全タグ deploy-nginx:full-20260826 内の
confから完全版(dify+実鍵入り・510行)を回収し復旧。以後の正本は
/home/ubuntu/jrtokai-phase12/deploy/nginx-new/nginx.conf(完全版+shirei・544行・実鍵入り)。

**Why:** リポジトリ版confは実鍵プレースホルダ(__EVAL_EXT_KEY__/__KG_INTERNAL_KEY__)+
ブロック欠落があり、EC2稼働版と二重管理になっている。
**How to apply:** nginxを再ビルドする前に必ず「docker exec jrtokai-demo-nginx cat /etc/nginx/nginx.conf」
と正本ファイルをdiffし、稼働版が新しければ先に正本へ取り込む。ビルド後は全10サブドメインの
疎通確認(dify=307が正常・他=401が正常)。nginx保全タグは2世代ローテ([[quant-standards-hb-t2]]の
ディスク教訓と同じ)。実鍵はgitへコミットしない(ローカル版はプレースホルダ維持)。
- 2026-10-22 決288: console の `/v1/eval/` を `return 404` に(鍵の値は設定から消えた)・`/v1-20260808_0525/` を削除。現行 nginx.conf sha16 bf11960ce6750eb3(2026-10-22 版)・退避 EC2 `~/nginx_backup/nginx.conf.pre-dec288-20261022`(鍵入り・600)。設定はコンテナ内のみ(マウント無し)なので docker cp で出し入れし `nginx -s reload`。

**2026-11-03 の訂正(決348 の追補 4・確定)**: 10/09(決223)から設定の変更は `docker cp` + reload になり、**ディスクの `deploy/nginx-new/nginx.conf` は 9/4 の版(sha16 1f69091c9df8ffe5・543 行)のまま更新されていない**。動いている版は bf11960ce6750eb3(649 行・決288 まで)で、**コンテナの中にしか無い**(旧の EC2 の退避は 10/09 の 9b5b42c55eb12025 まで)。ディスクの版で作り直すと、停止中の `jrtokai-demo-neo4j-v4` を参照して起動しない(新アカウントの複製で再現)。再起動・EC2 の再起動では消えない(コンテナの層に残る・決223 で起動時に名前を引かない)。今の版の写しは、2026-11-03 の AMI(ami-059dbdb4ed9e87526)と新の複製のディスク(IP だけ替えた版)にある。**How to apply(更新)**: 旧の nginx のコンテナを作り直さない。正本は「動いているコンテナの中」と読み替える。作り直しが要るときは、先に `docker exec ... cat` で取り出してディスクへ写してから。

**2026-11-04 の訂正(coord info 20261104-0007・上の 9/4 の記述より優先)**: 旧の EC2 の nginx の**動いている設定はコンテナの中にしか無い**(sha16 bf11960ce6750eb3)。ディスクの `deploy/nginx-new/nginx.conf` は 9/4 の古い版で、正本ではない。**コンテナを作り直さない**(`docker compose up` での作り直し・`docker compose build nginx`・`down` など)。作り直すと、停止中の `jrtokai-demo-neo4j-v4` を参照する行で起動できず、全部の画面と Dify が止まる。コンテナの再起動と EC2 の再起動は問題ない。設定を変えるときは、`docker cp` でコンテナの中へ入れ、`nginx -t` の後に reload する(決223 で resolver 化済み)。戻し方は coord の手順書 §0-6(AWS の移行の手順書)。関連: [[ec2-nginx-reload-after-container-restart]] / [[aws-account-migration-20261031]]
