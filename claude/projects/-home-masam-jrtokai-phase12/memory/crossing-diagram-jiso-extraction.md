---
name: crossing-diagram-jiso-extraction
description: 踏切制御図表の進行定位を幾何(輪郭太さ)で決定論判定し全94本の踏切図データ(キロ/型式/現示/進行定位194)を9990+EC2 cgraphへ加法投入・回帰合格。API露出は未
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-07-30T17:13:39.366Z
---

踏切制御図表の「図」から未構造化だった項目を追加抽出し 9990(関西 neo4j-crossing-v6)+ EC2 cgraph(jrtokai-v9-neo4j-cgraph)へ**加法投入**(2026-07-31)。[[added-data-integration-2026-07]] [[eval-and-table-comprehension]] の続き。

- **進行定位(最難関)を幾何で決定論判定**: ベクター線幅は一律(無情報)・信号名/キロはテキスト層皆無(CADベクター)。→ 高DPIラスタ化し cv2 HoughCircles で信号円検出、**輪郭ダーク帯の半径方向厚**を実測。進行定位=約1.0pt輪郭・停止定位=約0.71pt。実装 `kb_demo_v6/crossing_jiso_geom.py`(半径5.2-9.5pt@400dpi・信号帯 y∈[0.22,0.56]×高に限定でタイトル/諸元欄の文字誤検出を排除・厚み測定は半径rを含むダーク帯のみで隣接配線を排除・帯[0.88,1.35]ptで二重円/重畳異常を除外)。蟹江=正解[1RA/3L/12R/11LA]に4/4一致。信頼度フラグ clean63/review31。
- **役割分離(V6哲学)**: 太/細の判別=幾何(決定論)、記号テキストの読取=vision。太円に番号マーカーを描いた注釈画像をvisionに渡し記号だけ読む「注釈誘導OCR」(`read_jiso_symbols`)。
- **B案=注釈vision読取を進行定位の権威**: 全体抽出(crossing_diagram_llm)は信号命名が不安定(12Rをblock誤分類・1RA/1RB→1R,1R等)なため、太円の集中読取記号を正規化(併記/分割・信号パターン外[照X/PEB等]除外・中継括弧除去)して進行定位を確定。抽出漏れは幾何権威で補完(種別=「信号機(進行定位・幾何補完)」)。進行定位=true 194ノード(補完87)。
- **新スキーマ(専用ラベルで連動SignalPostと分離)**: `(:Crossing)-[:HAS_CDIAGRAM_SIGNAL]->(:CDiagramSignal{記号,種別,方向,現示種類,進行定位,進行定位_prov,進行定位_conf,kilo_post,source_layer='踏切制御図表'})`。加えて Crossing.kilo_post(105)・Station.kilo_post(13/20・駅名一致分)・ElectronicTrainDetector.型式(250, 例HC-31)・Marker.kilo_post。全て加法MERGE・prov_method='vision_llm_diagram+geom'。
- **パイプライン**: `batch_diagram.py`(拡張プロンプト再抽出94/94)→ `batch_merge_jiso.py`(幾何進行定位・再開可)→ `crossing_diagram_export_kg.py`(cleaned_jiso_syms でB案)→ 9990適用。HTML=`scripts/gen_table_verify_html.py`(CDiagramSignal追加・詳細列に進行定位/型式/現示種類)。
- **回帰合格**: try1×208 実効correct≈70(基準69)=非低下。悪化10件は全てSoundingRule/SignalPost/crossing_type設問でcypherは新ラベル不参照(別ラベルで既存MATCHに不可視)、再実行で5件復帰=LLM揺れと確定。9790=338不変。
- **EC2反映済**: cgraphへ同一cypher(CDiagramSignal削除+all94)を docker exec cypher-shell で適用。nodes4806→5705・進行定位194・蟹江正解一致。ssh=`kb-demo-ec2`(54.250.247.97)。cgraph pw=jrtokai2026test。

**API露出=完了(2026-07-31)**: kg_api/app.py の interlocking SCHEMA_HINTS を加法拡張(Crossing.kilo_post/Station.kilo_post/ElectronicTrainDetector.型式/CDiagramSignal+進行定位/現示種類、「進行定位=CDiagramSignal.進行定位でありgenji_cut(現示カット)とは無関係」を明記)。本番APIが正答するよう改善(蟹江: 進行定位=11LA/1RA/12R/3L・キロ6k552m・型式HC-31。旧はgenji_cut誤答)。再回帰try1×208=非低下(実効correct≈69=基準、incorrect 17→13改善、悪化はSoundingRule等の揺れで新ヒント不参照)。EC2 kg-api(jrtokai-v9-kg-api)へ app.py を docker cp+ホスト複製/home/ubuntu/jrtokai-v9/kg_api/app.py更新+再起動で反映・本番検証済。app.pyはbind-mountでなくcontainer内/app/kg_api/app.py。

**follow-up 1-3 完了(2026-07-31)**:
- FU1 方向決定論化: CDiagramSignal.方向を記号のL/R規則(R接尾=下り/L接尾=上り、字面『上り/下り』優先)で導出(`dir_of_sym`, 方向_prov='rule_記号LR')。蟹江8/8正解(vision誤りの13R上り→下り是正)。635中590が規則導出・45がvision fallback(61/201/51イ等L/R無)。軌道帯幾何より記号規則が確実(蟹江GT100%一致)。
- FU2 option C: 注釈vision併記読取("1RA/1RB")のマーカーのみ、その円周辺を±42pt高倍率クロップして1記号ずつ再読取(`_read_one_circle`, reread時のみ追加vision)。西蔵の過剰生成1RB/11LB解消。進行定位196。
- FU3 駅キロ: 未設定7駅=4駅(伊那北/伊那市/北殿/沢渡)は飯田線でnull正当、残3駅は vision駅名(桑名駅/白鳥信号場/朝日/長島)が連動Stationノード(桑名朝明駅/白鳥駅)と別地点のため強制一致せず(汚染回避)。現13駅が正しい=非fix。
- FU1+FU2を9990+EC2 cgraph両方へ再適用(CDiagramSignal 615/進行定位196)・本番API検証済(蟹江下り=13R/1RB/1RA/12R正答)。スキーマ不変(プロパティ値のみ変更)ゆえ回帰再実行不要。

**残limitation**: (1)review31本の進行定位は依然精度低め(conf='review')。(2)PDF94本 vs Crossing108=差14踏切は図表PDF無し。(3)方向規則はR=下り/L=上り(関西本線)を蟹江GTから採用=線ごとに向きが逆の可能性は要留意。
