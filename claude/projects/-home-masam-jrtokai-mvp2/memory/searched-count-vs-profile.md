---
name: searched-count-vs-profile
description: 読取の rep の searched の「未検索」の数は、プロファイルの phase1 の「未検索」の数と照らす・決54 で 14 と 7 の差を見落とした(2026-11-05・coord)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-05T05:46:22.495Z
---

読取の rep を取り込むときは、出力の `_meta.searched` で「未検索」の鍵の数を、承認済みのプロファイル(`profile_v0.json` の `phase1`)の「未検索」の鍵の数と照らす。数が多ければ、その差の鍵を一覧にする。

**Why:** 2026-11-05 の決54 で、coord は dgr の rep 1402 §3.1 の「未検索 14」を、プロファイルの「未検索 7」と照らさずに取り込んだ。プロファイルで「探す」の鍵のうち 7 つ(E3.kind・E3.decision・E4.struck・E4.defines_target・E5.ports・E12.corner・E12.merged)が、出力では「未検索(道具が出さなかった)」だった。次の acc の rep 1441 で 3 つが指摘されてから、決55 §3 の 2 で dgr に求めた。「未検索」と書くこと(K1)は正しくても、承認したプロファイルの探す範囲を黙って狭めることになる(CLAUDE.md §3 の 4)。

**How to apply:** 照らすには、合成の図の出力 `~/jrtokai-mvp2/data/derived/drawing/_synth/v2/<図>/phase1/read_v1.json` の `_meta.searched` を、プロファイルの鍵ごとの `phase1` と突き合わせる。合成の図は顧客データでないので、coord が読んでよい。実の図の出力は、値を出さずに鍵の名と状態だけを照らす。

関係: [[drawing-redesign-phase0]] [[read-whole-section-before-dec]]
