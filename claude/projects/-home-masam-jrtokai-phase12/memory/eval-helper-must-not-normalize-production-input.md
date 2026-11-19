---
name: eval-helper-must-not-normalize-production-input
description: 評価用の補助が入力を「きれいに」すると本番の入力とずれる — similar_query 改訂 0 が現象の文の NFKC と文末「。」外しで段 0 を本番と違う検索にした(2026-10-28・dkb が発見)
metadata:
  type: feedback
---

dkb のレポート向けに作った similar_query.build_query(改訂 0)は、現象の文にも答えの句と同じ清掃(NFKC・文末の「。」外し)を当てていた。本番の診断は利用者の文をそのまま部品へ渡す。部品は文末の「。」だけで上位 5 が 108〜270/646 問で変わるので、段 0 が本番と違う検索になった。さらに、dkb が段 0 だけを本番の文で取り直すと、段 0 と段 n の差に「。」の差が混ざる。改訂 1 で、段 0 は文をそのまま返し、段 n は文末の形を段 0 にそろえた。

**Why:** 足す側の句を整える清掃を、元の入力にも同じ関数で当てた。入力の正規化は「本番が何を渡すか」で決まるのに、それを確かめなかった。

**How to apply:** 評価や比較の補助で本番の口に渡す文を作るときは、段 0(何も足さない形)が本番の入力と byte 同一であることを selftest に入れる。比べる段のあいだで、比べたい差以外(句読点・全角半角・末尾の形)をそろえる。[[annotations-must-read-original-input]] [[report-from-the-consumer-side]]
