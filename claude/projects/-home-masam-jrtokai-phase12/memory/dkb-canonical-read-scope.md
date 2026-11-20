---
name: dkb-canonical-read-scope
description: dkb が手元の正本に繋いでよいのは B・D-KG・語彙 DB の読みだけ(利用者の決め 2026-11-18)— C の正本(neo4j-crossing-v6・neo4j-kb-v6)は読むだけでも ckb の持ち分
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-20T11:24:20.399Z
---

利用者の決め(2026-11-18・決351 追補 85〜88 の頃・[[b-canonical-conn-container-env]]): dkb = B の読みと第 1 弾の反映の書き込み・D-KG と語彙 DB の読みだけ。C の正本は入っていない。2026-11-20 20:3x に、C の数の形の比べの道具の出力の形を見るため、手元の C の正本(neo4j-crossing-v6)に読むだけの数の照会を 1 回当てた(書き込み 0)。範囲の外として coord が利用者に報告した。

**Why:** 正本への接続の範囲は、セッションごとに利用者が決めている。読むだけ・1 回でも、決めの外の接続は報告の対象になる。

**How to apply:** C の中身を見たい時は、ckb に頼むか、演習の台の使い捨ての C の写し(r351reh-<試行>-cgraph・ckb の書き出しを読み込んだ物)に当てる。道具の出力の形の確かめも同じ。正本の容器の名(neo4j-crossing-v6・neo4j-kb-v6・neo4j-accident-v7 など)に docker exec する前に、それが自分の範囲かを確かめる。
