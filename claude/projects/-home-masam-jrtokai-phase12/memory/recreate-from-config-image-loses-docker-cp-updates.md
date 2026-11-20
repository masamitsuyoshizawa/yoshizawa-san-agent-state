---
name: recreate-from-config-image-loses-docker-cp-updates
description: 容器を inspect から作り直すと、Config.Image(最初の像の名)に戻り、docker cp で写した更新が消える — 2026-10-28 に shirei のデモが 9-25 の版に戻り、3 週間気づかなかった(利用者の指摘 2026-11-19)
metadata:
  type: feedback
---

EC2 の容器を作り直す(recreate)時は、作り直す前に、容器と像の差と、中のプログラムの指紋を確かめ、いちばん新しい札の像から作る。作り直した後は、鍵と口だけでなく、プログラムの版(指紋)も確かめる。

**Why:** 2026-10-28 の鍵の交換(決329 段 B)で、coord が、6 つの画面の容器を `~/t327/recreate_env.py` で作り直した。この道具は容器の Config.Image(最初に作られた時の像の名)から作る。shirei は、反映のたびに docker cp で写し、その状態を新しい札(s7・v2label・s3・mermaid・t270)として commit する運用だったので、Config.Image は最初の sing-20260925 のままで、作り直しで 5 回の更新が全部消えた。確かめは、鍵の指紋と口の応答だけだった。2026-11-19 に利用者が「前の版に戻っていないか」と指摘して発覚。facets-v3 も同じ形で、データのファイル 1 つが 9-25 の是正の前に戻っていた。

**How to apply:**
- 作り直す前: `docker diff <容器>`(__pycache__ などを除く)と、`docker exec <容器> sha256sum <主なファイル>` を、同じ名の像のいちばん新しい札(`docker images --format '{{.CreatedAt}} {{.Tag}}' <repo> | sort -r | head`)の中身と比べる。違えば、新しい札の像を使う。
- 作り直した後: プログラムの指紋が、リポジトリの期待の版(コミット)と同じ事を確かめて、報告に書く。
- docker cp で反映した時は、commit した札から容器を作り直しておく(Config.Image を、いちばん新しい札にそろえる)と、次の作り直しで戻らない。
- 「前の版に戻っていないか」と聞かれたら、容器の作成の時刻・像の札・同じ名の像の新しい札・中のファイルの指紋を、順に見る。
- 関連 [[ec2-replace-deploy-config-in-files]] [[compose-env-canonical-kg-api-keys]] [[api-key-rotation-20261028]] [[derived-edge-drift]]


(索引から移した説明・2026-11-20)
- [容器の作り直しで docker cp の更新が消える](recreate-from-config-image-loses-docker-cp-updates.md) — 10-28 に shirei が 9-25 の版に戻り 3 週間気づかず(利用者の指摘 11-19)・作り直す前後にプログラムの指紋を照らす
