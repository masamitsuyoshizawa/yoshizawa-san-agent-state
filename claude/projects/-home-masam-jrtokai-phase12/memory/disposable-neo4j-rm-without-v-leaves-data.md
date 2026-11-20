---
name: disposable-neo4j-rm-without-v-leaves-data
description: 使い捨ての Neo4j の容器を docker rm -f(-v 無し)で消すと、像の /data・/logs が名の無い置き場で残り、B の写しが手元に残る
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-20T04:31:32.036Z
---

neo4j の像は /data と /logs を VOLUME に持つ。名前付きの置き場を結ばずに起こした容器を `docker rm -f` で消すと、2 つとも名の無い置き場として残る(ぶら下がり)。中には、読み込んだ B の写しの全部か一部が入る。2026-11-19〜20 に、dkb の調べ(r2・r3・r3b・材料作り・接続の確かめ)で 19 個を残した。うち /data は 7 個。CreatedAt(UTC)と中身の名の形で私の物と確かめて、消した(1,369 → 1,350)。

2026-11-20 に範囲が広いと分かった: 台の道具 `accident_kb_v7/scripts/rehearse_env.py` 自体が、容器を消す 4 か所とも -v 無しで、台を回すたびに 2 つずつ残していた。ほかに 23 のファイル(語彙・B・D-KG・C・fed の道具と試験・監査の audit_dkb.py)に 36 か所在り、-v 付きはコードの全体で 1 か所だけ。手元のぶら下がりは 1,346 個(できた月 2026-09 が 373・10 が 492・11 が 433)・Docker の集計で使われていない置き場 353 GB(coord の数え)。同じ秒に 2 つできた組が 655 組。mvp2 が「そちらの物か」と問うた 4 組は、台の回の記録の名の時刻(r351canon_restore_20261118-235331 など)と分まで合った。片付けは他 PJ の物が混ざるので、coord が利用者に諮る(決351 追補 158)。

**Why:** 伏せた B でも、写しが知らないうちに置き場に溜まるのは、個人情報の取り扱いとして良くない。ぶら下がりの数(手順書 §8 の注)も、演習が増やした分と分けられなくなる。

**How to apply:** 使い捨ての Neo4j は、`docker rm -f -v` で消す(名前付きの置き場は -v でも消えないので、名前付きを結んだ容器にも付けてよい)。新しく docker で容器を起こす道具を書く時は、`git grep '"docker", "rm"'` で同じ形が他に無いかも見る。台本の片付けでも、名前付きの置き場とは別に、名の無い置き場の数を前後で数える。消す前に、CreatedAt と中身の名の形で、自分の物かを確かめる。関連 [[dir-mode-0600-blocks-rig]]・[[neo4j-heap-exhaustion-on-repeated-bulk-loads]]。
