---
name: ckb-handover
description: 2026-09-12 dec 1300 で図表 KB(C-KG 9990)の所有が dkb → ckb セッションへ移った。引継書と dkb の残役割
metadata:
  type: project
---

2026-09-12(dec 20260912-1200/1300): 図表 KB(C-KG・9990・EC2 neo4j-cgraph・kb_demo_v6/layer23/kg_api v1)は専任の **ckb セッション**が所有(OWNERS 確定)。
引継書 = `knowledge_kb_v8/reports/audit_x16_c_20260912.md`(X16 初回監査: 手順のみ 11・乖離 12・担保案 ingest_diagrams.py G0〜G10 約 9 日・引継ぎ節)。
dkb が残した C 系の道具: `knowledge_kb_v8/scripts/rehearse_env_c.py`(ネットワーク分離の隔離演習)、`export_graph_apoc.export_driver` の elementId 方式(C/D-KG/B 往復一致)、9990 の初 export(0d2966fe5ddab5b6・ARTIFACTS)。
即時措置(coord→ckb): `run_full_pipeline.sh` の Cypher(全消去型)を 9990 に流さない。利用者判断待ち: id 規約の加法付与・`docs/export/cgraph_full.cypher` の git 追跡。

**Why:** dkb は歴史的に図表 KB を開発してきたが、役割分離で B / D-KG に専念する。
**How to apply:** C-KG の変更・演習・EC2 反映は ckb(req で依頼)。dkb の残役割 = B/D-KG の場所ノード是正(SAME_PLACE_AS・顧客確認待ち)と `docs/監査付録_B-DKG系.md` の維持・`ingest_pipeline` commit モードの運用。関連: [[ingest-pipeline-enforcement]] [[dkb-recurrence-improvement]]
