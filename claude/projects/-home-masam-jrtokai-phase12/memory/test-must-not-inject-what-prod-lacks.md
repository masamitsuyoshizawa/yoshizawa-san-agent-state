---
name: test-must-not-inject-what-prod-lacks
description: 試験で名前空間に注入したものは本番に無いかもしれない。注入をやめて本番と同じ条件で呼ぶ
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-17T00:23:06.837Z
---

`kg_api/kb/scripts/test_*.py` は `app.py` の関数を `exec` して動かすため、名前空間に
`collections`・`hashlib`・`re`・`unicodedata` などを**注入**していた。**注入したものが本番の
`app.py` にも在るとは限らない。**

2026-09-18 に本番を落とした。`_symbol_terms` が `unicodedata` を使うのに **`app.py` の
モジュール直下に import が無く**(20 箇所すべて関数内 import)、`/ask`・`/query`・`/nl2cypher`
の 3 入口が `NameError` で **HTTP 500**。試験は注入していたので通っていた。**`urlparse` で
同じ型を踏んだ後、記憶に残していながら再発させた。**

**正しい形**: **`app.py` のモジュール直下の `import` 文を構文で集め、それだけを渡して実際に呼ぶ。**
DB など外部だけを合成に差し替え、**それ以外は渡さない**。名前ごとに
`^from urllib\.parse import .*\burlparse\b` のような正規表現を足す形は、**次の名前でまた漏れる**。

**握りつぶしに注意**: `except Exception` で捕まえて `error` に記録する関数(語彙の構築など)では
`NameError` も捕まるので、呼ぶだけでは落ちない。**`error` が None であることも併せて見る**
(`hashlib`・`collections` を外す変異で最初は落ちなかった)。

**Why:** 試験が本番より寛容だと欠陥を隠す。今日だけで 3 度踏んだ — 合成語彙の件数基準
(完全一致 vs `CONTAINS`)、本番の呼び出し順序(単体で呼んで数珠つなぎを見逃した)、名前空間への注入。

**How to apply:** 検査を書くときに「本番と同じ条件か」を先に問う。**注入しているものが 1 つでも
あれば、それは本番に無いかもしれない。** 関数内 import を大域と仮定する新しいコードにも注意。
関連: [[claim-wider-than-implementation]] [[annotations-must-read-original-input]]
[[negative-test-check-reason]]

## 追記(2026-10-09・決212 の 422 の形)
- 試験で合成した応答の形({"detail": {...}})が本番の app.py の包み({"error": {...}})と違い、EC2 では固定文と印が出なかった(bkb・fed の試験が同じ形で合成・coord の A4 も `detail.code` が None と出ていたのに見落とした)。**応答の形は本番の口を 1 回叩いて写し、その現物を試験の固定値にする。** 「code が None」のような空の値は合格の印ではなく形の不一致の合図。
