---
name: vocab-alias-source-b-is-count
description: "語彙 DB の Alias.source の \"B:N\" は事故 ID ではなく B での出現数・別名に支持事故 ID は無い(explore 改訂 4 で判明)"
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-12T15:48:01.250Z
---

語彙 DB(neo4j-vocab-v3・10190)の `Alias.source` にある `B:N` は、B での出現の数(`build_dict_equipment.py` 314 行ほかが `f"B:{cnt}"` で書く)。事故 ID ではない。2026-10-13 に確かめた。

- 別名 2,283 のうち、出どころが B 由来の語(B:N・llm_context・llm・cooccur)だけのものは 1,380。
- 既存の `knowledge_kb_v8/data/eval/static_support.json`(3 層辞書用・2026-09-15)は、B:N を含む別名 1,437 のうち 327 しか覆わない。
- 支持した事故を出すには、`build_static_support.py` と同じ本文一致の推定(LOO700 の本文・4 字以上)しかない。推定であることを明記する。

**Why:** explore の設計書 改訂 1〜3 で「支持事故が除外に入る別名を使わない」と書いたが、ID が在る前提を確かめていなかった(rev R232-6a の棚卸しで露見)。[[assumed-current-state-without-checking]] と同じ型。

**How to apply:** 語彙・辞書に LOO の静的除外を掛ける設計を書く前に、出どころの欄の値が何か(ID か数か)を生成コードで確かめる。[[check-where-a-value-can-be-read-from]]
