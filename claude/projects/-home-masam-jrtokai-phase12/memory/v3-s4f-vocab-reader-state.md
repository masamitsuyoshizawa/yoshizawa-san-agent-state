---
name: v3-s4f-vocab-reader-state
description: V3 第 2 段 S4-F(fed の語彙 DB の読み手)は閉じた・3 者照合と交錯試験に合格・次は S5-4(2026-09-24)
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-24T04:30:49.987Z
---

**2026-09-24 に S4-F が閉じた**(coord)。fed の読み手 `kg_api/sources/vocab_db_reader.py`(sha16 `b09ce51a270d6737`・fed コミット 1fdf904d)・試験 `kg_api/kb/scripts/test_vocab_reader_v3.py`(`f0a815be4de0aaf2`・35 件)・probe `kg_api/kb/scripts/probe_vocab_reader_v3.py`。

- **典拠は設計書 `docs/設計_V3_語彙DB_20260921.md` の文だけ**(いま追補版 12 `47b3d25645d5ab55`)。**dkb の `knowledge_kb_v8/scripts/vocab/vocab_db.py` は開かない**(2 実装の独立性)。試験の D0 が設計書の sha を持つので、**追補版が上がると D0 が落ちる** → D1〜D5 を新しい文と照らしてから上げる。
- 正本の語彙 DB(bolt 10190・`neo4j-vocab-v3`・R1 = v3.0)の束の digest `2e763fbae25c6a3d`(7,840 / 9,549 / 17,389)を独立に再現。**bkb の 3 者照合で 17,389 要素一致**・**交錯試験(第 2 相を挟む)で読み直して R2 の束を返し合格**(bkb rep 1330)。
- 札 17(設計書 1418 行)。**`unknown_property` は設計書でなく fed の計画の名だった**(誤って「設計書の名」と書き訂正した)。
- この DB は read committed → **読み終わりに P を読み直す**(決55)。上限 3 回・尽きたら `ReadRefused("BUNDLE_RACE")`(決56)。
- 接続は `kb_conn` の `vocab`(`KB_VOCAB_NEO4J_URI` 等)。読むたびに LOCKS と info。

**S5-4 済み(2026-09-24・fed コミット 3842a3b0)**: `KG_VOCAB_SOURCE=db` で DB 経路(既定 JSON・切替は S7)。束 → JSON の entries / surface_map を組み直して同じ `_absorb` に通す(DB に並びが無い — JSON 経路が並びに依存しないのは 6,494 語 × 3 通りの実験で確認・証明ではない)。失敗は 503・束を置かない。凍結試験 186 件は実 DB の生の行 `fed_raw_v3.0_r1.json`(419b395544663fad)を再生する。**本番の接続の道は session を差し込む試験では通らない** — 実 DB で 1 回通した。

**S7 完了(2026-09-24・決63)**: EC2 の kg_api は語彙 DB の経路(`s7b-20260924`・案 B = 設定ファイル `kg_api/kb/config/vocab_source.json`・秘密値は `KG_NEO4J_PW` への後退)。配置の版 = fed コミット 942c6c74(facet_backend `dfe4ce3932405464`)。**設定ファイルをローカルの kb/config に置くとローカルも DB 経路になる**(配る物は `deploy/kg_api_s7/`)。API 仕様書は S7b 後で作り直し(`ac3fdd0bf04c5191`・例は EC2 の応答)。**fed の第 2 段の手番は終了**。残り = bkb の凍V-c・共通表の次の改訂・旧 v3. URL の除去(10/08・coord)。

**共通表の確定版は 第 3 版 改訂 11 `30ba93598a6da687`(842 行・決66・2026-09-24)**。以後は「§n + 改訂 11 30ba9359」で引く。§1-17 に第 2 段の完了状態と現行版。

**(以下は S5-4 前の残りの記録)** **残り(S5-4 で)**: 束を `_absorb_entries` と同じ形(+ `parent_mark`)に写す部分・`load_vocab` からの切替(環境変数・既定は JSON)・meta の `vocab_release` / `vocab_graph_digest` を埋める・尽きたときの 503 相当・(E16)〜(E27)・F13 の往復を Equipment 以外の 6 型へ・実 DB の F8/F9。

**Why:** S5-4 を始めるときに、読み手の版と独立性の規律と残りを取り違えないため。
**How to apply:** S4 終了の dec を確かめてから S5-4 に入る。関連: [[v3-stage2-s3-plan-state]] [[precommit-hash-for-independence]] [[attribution-claims-need-checking]]
