---
name: storage-order-changes-dkb-responses
description: 同じ graph_digest の B でも書き出しから戻した DB では D-KB の応答が 70/646 変わる(保存の順に依る照会)・DB 間の比較は保存の順をそろえてから(2026-09-25)
metadata:
  type: project
---

2026-09-25 の決97 (2) の測定で分かった(rep 20260925-2210)。書き出し(export_graph_apoc の driver)から一時の Neo4j に戻した B は graph_digest が正本と一致するのに、D-KB の応答が LOO 646 件中 70 件で正本と違った。S5 の前後どちらでも 70 件なので中身の差ではなく保存の順(ORDER BY の無い照会など)と推定。

- 最初「戻した S5 前 対 正本の S5 後」で比べて 74 件と出したのは測り方の誤り。戻した DB どうし(保存の順をそろえた)で比べると 7 件で、これが中身の効果(1 位・順位の変化 0)。
- **EC2 とローカルは実測で 646/646 一致**(2026-09-25 22:40 頃・トンネルで B・D-KG とも)。「1 割ほど違う見込み」と書いたのは誤りで、書き出しから戻した写しの結果を別の手順で作られた EC2 に当てはめた推定だった(rep 2303 で訂正)。依存そのものは在り、作り直した環境では出る。
- 答えに効くのは dkg_backend の 4 か所(概況照合・再発照合の 2 本目に ORDER BY なし・再発照合の見出しの順と同点・_b_example_causes の ORDER BY が確度だけ)。案 = 計画_B照会の順の明示 e229e68a(実装は別承認)。
- 他人のトンネルを借りて測ると閉じられて止まる(実際に起きた)。自分で開き pid を控え、終わったら閉じる。

**Why:** digest の一致を「同じ動作」と読むと、保存の順に依る照会の差を見落とす。
**How to apply:** DB を戻して前後を比べるときは、両側とも同じ手順で戻した DB どうしで比べる。環境間の一致を言うときは応答でも見る。照会の順の明示は応答が変わるので LOO 前後比較と別承認。関連: [[hash-match-not-completeness]] [[bkb-koh13-outcome]] [[isolate-the-change-before-blaming-version]]

## 統合した記憶: dump-load-preserves-storage-order(2026-10-19 に索引の行数を減らすため統合)

語彙 DB v3.0 の dump を使い捨てのコンテナへ load し、Equipment・Alias・SYNONYM_OF・HAS_PARENT を `ORDER BY` なしで読むと、4 種とも正本と並びが完全に一致した(位置の違い 0)。

**Why:** D-KB の順の件で差が出たのは export から作り直した DB だった。dump/load は束 digest の一致を確かめる複製には使えるが、保存の順を変えることはできない。bkb の受K4 の d2 は、まさにこの前提で組まれていた。
**How to apply:** 「保存の順が違う DB」が要る試験では、dump/load を使わない。論理の値から逆順に書き込んで作り直し、作った後に並びが違うことを実測してから使う。関連 [[storage-order-changes-dkb-responses]] [[facets-instance-entry-issue]]

**逆順の書き込みの道具(2026-09-30・決152 (b))**: `knowledge_kb_v8/scripts/vocab/vocab_reverse_write.py`(f39cf7a8)。正本の全ノード・全辺を順序を指定せずに読み、空の一時コンテナへ逆順に書く。同じラベルが続く区間ごとに UNWIND でまとめ、全体の順は保つ。4 種の並びがすべて正本と違い、束 digest と全件 hash は同じだった。d2 は成り立つ。一時コンテナ neo4j-vocab-d2(18694)は bkb の B2 の後に `--drop` で消す(LOCKS 保持中)。
