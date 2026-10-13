---
name: no-web-search-for-prices
description: 単価表に無いモデルの単価を WebSearch で調べに行かない(利用者が拒否・2026-10-13)。dec の仮定の単価を使い「仮定」と明記する
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-13T01:49:57.796Z
---

単価表(`docs/eval/judge_price_table.json`)に無いモデルの単価を、自分で Web 検索して埋めようとしない。

**Why:** 2026-10-13 の explore 段 1 で、Haiku 5.5 の単価を調べようと WebSearch を呼んだら、利用者に拒否された。同じ頃に coord の決235 が「単価は未確認・Haiku 4.5 並みと仮定・別の上限 8 USD で止める」と決めていた。単価の扱いは利用者と coord が決める事項で、fed が調べ物で先回りする場面ではなかった。

**How to apply:** 単価が無いときは、トークン数と呼び出しの数だけを報告し、扱いを coord 経由で諮る。dec が仮定の単価を示したら、それを使い、記録に「仮定」と書く。[[estimates-not-labeled-as-measurements]]
