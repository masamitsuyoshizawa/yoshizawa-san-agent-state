---
name: raw-diagram-api
description: 関西線の図表RAW(グラフ化前の構造化JSON)を Neo4j RawDiagram ノードに格納し kg_api /v1/raw で提供
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
---

グラフ化前の抽出RAW(連動図表・踏切制御図表から読み取った構造化JSON)をAPI経由で渡せるようにした(2026-07-15)。案A採用=Neo4j格納(単一バックエンド・来歴リンク・git外)。

- スキーマ: 新ラベル `RawDiagram`(9990のみ・非破壊・氏名なし)。id=`kansai:raw:{crossing|rendo}:{名前}:{layer}:{ファイル識別子}`。プロパティ=type/layer/name/source_pdf/extract_method/item_count/raw_json(JSON全文)/prov。関係=`(:Crossing)-[:HAS_RAW]->(:RawDiagram)`・`(:Station)-[:HAS_RAW]->(:RawDiagram)`。
- レイヤ(全レイヤ対等): 踏切=`制御表`(crossing_layer1: spec/rules/notes)94・`図設備`(_diagram_all_kansai.jsonl: 特発/障検/しゃ断機/受光器/閉そく信号/種別)94・`特発多数決`(emit 3パスvision)94。駅=`連動表`(rendo_kansai: 鎖錠系5欄のrows)16・`連動表マーク多数決`(lock 3パスvision)16。計 RawDiagram **314**、HAS_RAW **320**(同名踏切ノードのdup連結含む)。9790不変338。
- 投入: `kb_demo_v6/ingest_raw_diagram.py`(制御表/図設備/連動表)+`kb_demo_v6/ingest_raw_multipass.py`(特発多数決/連動表マーク多数決。commit d5f729b)。neo4jドライバのパラメータ格納=大JSONのエスケープ不要。idにファイル識別子(crossing_file等)=須成踏切の同名2ファイル衝突を回避。
- API(`kg_api/app.py`): `GET /v1/raw`(一覧, ?type=)、`GET /v1/raw/{crossing|station}/{名前}`(?layer= で単一レイヤ)。x-api-key認証。RAWはリクエスト時にNeo4jからライブ取得=データ追加にuvicorn再起動不要。注意: `?layer=`の日本語値はURLエンコード必須(未エンコードだとuvicornが'Invalid HTTP request')。
- 3パス中間RAWは**格納済**(2026-07-15): `特発多数決`(踏切、pass1/2/3のemitters)・`連動表マーク多数決`(駅、pass0/1/2のrows)。抽出元は`kb_demo_v6/data/raw_vision/`(emit_p1/2/3.json・lock_3pass.json。gitignore済)。定反/進路鎖錠の3パスRAWは未格納(確定値はグラフ本体、監査はdocs。要時は抽出再実行→永続化で追加可能)。
- 関連 [[added-data-integration-2026-07]]。
