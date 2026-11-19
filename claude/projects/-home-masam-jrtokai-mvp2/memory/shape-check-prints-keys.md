---
name: shape-check-prints-keys
description: 読取の出力の「形だけ」を見る確かめで dict の鍵を出すと、鍵が eid などの値になりうる・鍵の数と型だけを出す(2026-11-02・coord・b08e の eid_map)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-01T15:11:23.640Z
---

dgr の kgb_input の `_meta.eid_map` が空かを確かめるとき、「形だけ」のつもりで dict を `{鍵: 型}` に写して出した。eid_map は鍵そのものが要素の eid(型と番地を含む)なので、凍結の前の b08e の eid が coord の画面に 1 つ出た。連絡文と docs には書かなかった。

**Why:** 対応表・索引・集計の dict は、鍵が読取の値になる。値を伏せても、鍵を出せば値を見たのと同じになる。
**How to apply:** 凍結の前の図の出力の形を確かめるときは、`len(d)`・空かどうか・値の型だけを出す。鍵を出すのは、鍵が欄の名だと分かっている dict(`_meta` の頭の段など)に限る。[[acceptance-independence-no-element-kinds]] と [[acc-a-layer-values-before-freeze]] の、coord の側の手当て。
