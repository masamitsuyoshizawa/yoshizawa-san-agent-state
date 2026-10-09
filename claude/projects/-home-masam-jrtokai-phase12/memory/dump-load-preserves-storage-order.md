---
name: dump-load-preserves-storage-order
description: neo4j-admin database dump/load は物理の写しで保存の順も写る。ORDER BY なしの読み出しの並びは正本と完全一致する(語彙 DB で実測 2026-09-28)。順に依る実装の試験には使えない
metadata:
  type: feedback
---

語彙 DB v3.0 の dump を使い捨てのコンテナへ load し、Equipment・Alias・SYNONYM_OF・HAS_PARENT を `ORDER BY` なしで読むと、4 種とも正本と並びが完全に一致した(位置の違い 0)。

**Why:** D-KB の順の件で差が出たのは export から作り直した DB だった。dump/load は束 digest の一致を確かめる複製には使えるが、保存の順を変えることはできない。bkb の受K4 の d2 は、まさにこの前提で組まれていた。
**How to apply:** 「保存の順が違う DB」が要る試験では、dump/load を使わない。論理の値から逆順に書き込んで作り直し、作った後に並びが違うことを実測してから使う。関連 [[storage-order-changes-dkb-responses]] [[facets-instance-entry-issue]]

**逆順の書き込みの道具(2026-09-30・決152 (b))**: `knowledge_kb_v8/scripts/vocab/vocab_reverse_write.py`(f39cf7a8)。正本の全ノード・全辺を順序を指定せずに読み、空の一時コンテナへ逆順に書く。同じラベルが続く区間ごとに UNWIND でまとめ、全体の順は保つ。4 種の並びがすべて正本と違い、束 digest と全件 hash は同じだった。d2 は成り立つ。一時コンテナ neo4j-vocab-d2(18694)は bkb の B2 の後に `--drop` で消す(LOCKS 保持中)。
