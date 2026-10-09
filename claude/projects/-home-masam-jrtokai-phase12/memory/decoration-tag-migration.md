---
name: decoration-tag-migration
description: 連動図表の文字飾りを接頭記号○◎□からcamelCaseタグ<circle>/<doubleCircle>/<rectangle>へ移行済(9990・回帰合格)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-07-30T13:20:45.847Z
---

連動図表の文字飾り(囲い記号)の内部表記を、接頭記号 `〇/◎/□` から **camelCaseタグ `<circle>/<doubleCircle>/<rectangle>`** へ移行(2026-07-30完了・利用者承認)。踏切制御表は従来から `<circle>` 使用で、これに統一。**タグは形状のみ表し意味は下流で解釈**: 連動で circle=反位・doubleCircle=総括制御・rectangle=鎖錠対象、踏切鳴動条件で circle=進行現示。規約=`docs/文字飾りタグ規約_20260730.md`、対象カタログ=`docs/連動図表_文字飾りカタログ_20260730.md`。

- **コンバータ**(Phase A): `kb_demo_v6/normalize_decorations.py`(9990のみ・`KG_CGRAPH_BOLT`環境変数・9790 assert・冪等・`SET +=`加法・原値`_raw`保持・件数不変)。対象=`route_symbol`(SignalLight/Route:全体包み)・`section_lock_raw_notation/semantic/secondary_switches`(Route:転てつ器番号トークンのみ)。**ローカル9990とEC2 cgraph両方に適用済(各46件)**。非対象=`○標`(用語)・`●黒丸`(図記号説明)・id/name。
- **重要な設計**: 一致キー(id・エッジ`IN_ROUTE_OF.route`・lname 例`11L-〇A`)は**旧記法のまま不変**。表示/意味プロパティ`route_symbol`のみタグ化。cypherプロンプトは「route_symbolで直接比較せずid/エッジrouteで一致」を指示(だから回帰が通る)。
- **Phase B**(born-tagged): 9990書込み境界のみ変換=`rendo_export_kg.py`(関西・route_symbol)・`copy_v6_graph.py`(飯田線9790→9990複製時)。壊れやすいlayer1/build_layer3の〇依存regexは不変。layer1 JSON自体のタグ化は後方互換テスト要で意図的に後続(プロンプト両形対応済で機能上不要)。
- **Phase C**: `nl_pipeline`(kg_api/kb_demo_v6/v5)囲い記号説明をタグ表記(旧接頭形も同義と併記=移行的)、`knowledge_kb_v8/demo/app.py _circ`が⭕/◎/▢へ整形。SCHEMA_HINTSの○囲み言及は原表抽出規則の説明のため据置。
- **Phase D 回帰合格**: try1×208 correct 69→69(非低下)・c+p 121→122・incorrect 17→16・9990件数不変(4806/9669)・9790=338+`<circle>`混入0。
- 現データに`◎/□`は皆無=将来の対象記号増加に備えた新設。コミット b4acee6/2f10da8/ae752a3/5bd0447。

関連 [[eval-and-table-comprehension]] [[v9-api-schema-unification]]。
