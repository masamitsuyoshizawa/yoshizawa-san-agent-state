---
name: check-both-branches-of-a-condition
description: 条件分岐のある前処理は経路ごとに「掛かるもの」を列挙する(掛からないの確認だけでは足りない)
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-18T01:31:34.924Z
---

条件分岐のある前処理を説明するときは、経路ごとに**掛かるもの**を列挙する。「この経路では掛からない」を確かめただけでは足りない。可能なら経路に依らない性質で書く。

2026-09-18 に coord と ckb が対の誤りを同時に犯した。`kg_api/app.py` の `ep_ask` で `question_digest` を「**併記後**の質問の sha」と説明したが、`_annotate_accident` は `if graph == "accident"` の下なので C 系(interlocking)では掛からない。ckb がこれを「**併記前の生の**質問文」と訂正したが、その 3 行上に `if graph == "interlocking": _normalize_kanji(...)` があり「生」でもなかった。正しい言い方は「その入口・そのグラフでの前処理をすべて終えた、実際に送信へ回る質問文の sha」。

**Why:** 片方の分岐に無いことを確かめても、もう片方に在るものは見えない。同じ画面に映っていても、探している条件と違う条件は読み飛ばす。coord 側の型は「他人の説明を、条件の違う場所へそのまま持ち込む」で、材料(`_normalize_kanji` が interlocking 限定)は自分で前の連絡文に書いていた。

**How to apply:** 前処理・正規化・併記のような「値を書き換える処理」を仕様や手順書へ落とすときは、`if` の数だけ経路を数え、各経路で掛かる処理を並べてから書く。説明が経路ごとに変わるなら、経路に依らない性質(「実際に送られたもの」)で言い換える。[[annotations-must-read-original-input]]・[[claim-wider-than-implementation]]・[[verify-what-the-target-actually-reads]] と同じ系統。
