---
name: ec2-nginx-conf-canonical
description: EC2 nginx.confの正本は /home/ubuntu/jrtokai-phase12/deploy/nginx-new/nginx.conf(実鍵入り完全版)・再ビルド前に稼働版との差分確認必須
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
