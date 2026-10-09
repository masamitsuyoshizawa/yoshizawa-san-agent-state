---
name: prompt-hints-apply-to-every-request
description: SCHEMA_HINTS に足した行は条件節に見えても全リクエストに入る(効くか否かとは別に、プロンプトは変わる)
metadata: 
  node_type: memory
  type: reference
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-16T11:09:40.040Z
---

`kg_api` の `_nl2cypher_prompt_parts` の固定部は
`"# グラフスキーマ\n%s\n\n%s\n\n" % (schema_text(graph), SCHEMA_HINTS.get(graph, ""))` で、
**`SCHEMA_HINTS` はそのグラフの全リクエストに入る**。「質問に◯◯の併記があるときは〜」と条件節で
書いても、**併記が無い問いのプロンプトにも本文は必ず入る**。これは構造の事実。

**因果は別に測ること。** 2026-09-27 に (3) 故障様式の経路でこれが退行の原因と疑われ、転てつ機の述語
Recall が 0.6071 → 0.3214 と「2 対 2 で版の境目と一致」した。**しかし後に coord が、疑う変更だけを
外した版で反復して 0.3214 と 0.6071 の両方を得た。値はゆらぎで、退行は起きていなかった**(dec
20260927-1200 で撤回)。**機構が在ることと、それが観測された差を生んだことは別である。**

**Why:** 条件つきの指示でも常時記載はプロンプトを変えるので、「効かないのだから影響しない」とは
言えない。ただし「変わる」から「悪くなった」を導くこともできない。

**How to apply:** 問いによって効く/効かない指示は、**質問への併記側に畳む**と対象外の問いの
プロンプトが完全に同一になり、そもそも疑う必要が消える(A 案・dec 20260927-0900 で採用)。
「影響しない」と言うなら byte 同一で示す。検査は「対象外の問いで**質問文字列が一字も変わらない**」
まで見る — 語の抽出だけを見る検査は、指示行を削っても通ってしまう。
関連: [[isolate-the-change-before-blaming-version]] [[claim-wider-than-implementation]]
[[negative-test-check-reason]]
