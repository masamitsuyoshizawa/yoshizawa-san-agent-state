---
name: skill-consolidation-file
description: 図表KBの動作根拠(スキル集約)ファイルを継続維持する(利用者依頼)
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-08T13:16:03.335Z
---

図表KB(C・interlocking)の図表読取/Cypher作成/最終回答の全スキル・ヒント・実出典を集約した「動作根拠」ファイル `docs/図表KB_動作根拠_スキル集約_20260808.md` を新設。利用者は**今後も忘れず維持管理する**ことを求めている。

**Why:** ヒント/知識が app.py(SCHEMA_HINTS/format_answer)・glossary.json・docs/信号記号読取ルール・docs/踏切制御表_意味ルール・決定論注入関数に分散しており、動作根拠を一元把握する索引が必要。

**How to apply:** SCHEMA_HINTS/format_answer/glossary/決定論注入(sounding_readdown_ctx・hazard_stops_ctx・clearance_x_ctx 等)・凡例(F-1/R-2/R-3/R-4)を変更・追加したら、同ファイルの該当節と§5-6(固有記号埋込監査)を更新する。固有記号(TC/駅名/踏切名/具体値)の例示埋込は複写リスクとして§6で管理し、決定論算出へ委譲する方針。関連: [[eval-and-table-comprehension]] [[crossing-sounding-readdown]] [[legend-extraction]]。
