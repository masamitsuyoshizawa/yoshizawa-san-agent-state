---
name: ec2-nginx-reload-after-container-restart
description: EC2 nginx は決223(2026-10-09)で resolver 化済み — upstream の restart で reload 不要・止めたコンテナ名が残っても起動可。新 vhost を足すときだけ -t + reload。それ以前は 502 の原因だった
metadata:
  type: project
---

2026-10-09: 表示改善 第 3 段の反映で `jrtokai-v9-dev-console`・`jrtokai-shirei-demo` を `docker restart` した後、ブラウザから console/shirei が 502 Bad Gateway(利用者の報告・22:3x)。原因 = `jrtokai-demo-nginx`(5 週間稼働)が `proxy_pass http://<コンテナ名>:port` の名前を起動時に解決して保持し、restart で変わった IP(172.18.0.2/3)を追わない。EC2 内の `curl 127.0.0.1:8510` は 200 なので、コンテナ自体は健康。

**How to apply:** EC2 で upstream のコンテナ(kg_api・console・shirei・v9/v10・Dify)を restart/作り直した手順の最後に、必ず `docker exec jrtokai-demo-nginx nginx -t && docker exec jrtokai-demo-nginx nginx -s reload` を入れ、`curl -k --resolve <host>:443:127.0.0.1 https://<host>/` で 200 を確かめる。EC2 反映の手順書の smoke に「ブラウザ経由(nginx 経由)の 200」を足す。kg_api も同じ nginx を通る口(api.*)があるなら同様。

## 追記(決223・2026-10-09 22:47 JST)
- 案 A を反映: http/stream に `resolver 127.0.0.11 valid=10s ipv6=off`・名前形 proxy_pass 29 本と URI 形 3 本(rewrite)・stream を変数形に。稼働版 sha16 9b5b42c55eb12025。V4 = IP を変えた console がブラウザから開けた(利用者確認)。
- 以後: restart 後の reload は**不要**。新しい vhost/location を足すときは `-t` + `reload`。conf はコンテナ内のファイル(bind mount でない)・変更は docker cp + reload・退避は EC2 ~/nginx_backup_20261009 と手元 S。リポジトリ deploy/nginx-new/nginx.conf は鍵を伏せた写し。
- `grep -c "set \$up"` はダブルクォートで $ が行末になり 0 を返す → `grep -cF 'set $up'`。
