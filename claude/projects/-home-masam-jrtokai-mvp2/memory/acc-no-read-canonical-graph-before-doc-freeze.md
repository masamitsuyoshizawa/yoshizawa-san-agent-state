---
name: acc-no-read-canonical-graph-before-doc-freeze
description: acc は文書系の期待を凍結するまで、正本 neo4j-mvp2-kg の中身・K8 の書き出し・状態ファイルを読まない(決34 §4)・b08e の凍結までは図面の kgb_input と図面の分も読まない(決35 §4)・ダイジェストは読んでよい
metadata:
  node_type: memory
  type: project
  originSessionId: b54d92f6-694f-48da-9c1b-bc1712afcb52
  modified: 2026-11-01T06:11:56.064Z
---

決34(`20261101-1509_coord_to_kgb_dec_first-canonical-ingest-with-guards.md`・2026-11-01)で、kgb が正本へ最初の投入をする。正本には、dcr の読取から作った値が入る。

acc は、文書系の期待を凍結するまで、次のものを読まない。
- 正本 `neo4j-mvp2-kg` の中身
- K8 の書き出し(`data/graph/export/`)
- 状態ファイル(`data/graph/pipeline_state/`)
- グラフの台帳(`data/graph/graph_registry.json`・kgb の info 1517 で足された)

連絡文に出る正本のダイジェストだけは、読んでよい。

**図面にも当てる**(決35 §4・2026-11-01): dgr が図面 3 枚の kgb_input を `data/derived/drawing/<sha16>/kgb_input/` に書き出した。状態の置き場と K8 の書き出しには、図面の分も入りうる。b08e の期待を凍結するまで、b08e の kgb_input と、状態の置き場・K8 の書き出しの図面の分を読まない。決37(2026-11-01 23:25)で、演習の記録 `data/graph/pipeline_state/rehearse/` の図面の分も同じく読まない(info のダイジェストは読む)。**b08e は 2026-11-04 01:44 に凍結した(info 0145)ので、図面の分(dgr の b08e の出力・b08e の kgb_input)の読まない約束は解けた。ただし状態の置き場・K8 の書き出し・演習の記録・台帳は文書系の分を含むので、文書系の凍結まで読まない。**

**Why:** 受入の独立(CLAUDE.md §4)。`data/derived/` を読まない約束と同じ型で、グラフに入った読取の値も、凍結の前は見ない。

**How to apply:**
- 照合(C 層)や文書系の道具の計画で、グラフや書き出しを入力にしたくなったときは、凍結の後に回す。
- 図面が正本に入った後(kgb の info 1642 で投入を諮る入力がそろった・2026-11-04)も、正本には文書系の値が入っているので読まない。C 層の図面の分は dgr の出力(read_v0.json と kgb_input)を入力にし、版を記録する。
- kgb の info・rep は、凍結の前は数字を伏せて読む(件数の形が受入の節の数を示しうる)。

関係: [[acceptance-independence-no-element-kinds]]・[[check-counts-by-type-leak-acceptance]]
