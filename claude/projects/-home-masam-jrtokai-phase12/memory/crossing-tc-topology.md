---
name: crossing-tc-topology
description: 踏切制御図表から軌道回路トポロジー(配線)を抽出・加法グラフ化・API露出(完了・コミット済)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-02T14:39:51.456Z
---

踏切制御図表(全94本、`kb_demo_v6/testdata/20260703追加_踏切制御図表/*.pdf`)から軌道回路(TC)の空間トポロジーを抽出し、9990/EC2 cgraph へ加法投入した(2026-08-02 完了、コミット bb56dc7)。

抽出=`kb_demo_v6/crossing_tc_topology.py`(vision: TC並び=名古屋方→亀山方=下り昇順、踏切のTC所属、境界信号)。判別・統合は決定論:
- 境界信号は各踏切図の自信号ノード(CDiagramSignal/BlockSignal 記号)と突合し、制御子の単独英字(D/F/W/C/V 等)を除外(信号記号 下り1/上り3/1RA/12R は路線全体で一意でないので**グローバル突合は禁物**=各図ローカルで突合、キロ近似も踏切キロのみ使用)。
- NEXT_TC方向は読み順から確定(キロ不要)。局所並びをTC名でMERGEしてスティッチ(近傍図で重なり反復)。

新スキーマ(加法、連動表由来TCとは別体系。source_layer='踏切制御図表')。**重要: TC名(12RT/3LT/61T等)は各駅で再利用されるためグローバルMERGE禁物**(初版はグローバルで12RT=11踏切42km過融合)。是正版は**図単位スコープ** id=`kansai:tc:{踏切}:{TC名}`(利用者選択、コミット3ca5142)。同一図内で辿る前提(id STARTS WITHで横断しない):
- `(:TrackCircuit)` 634、`(:Crossing)-[:LOCATED_IN_TC]->()` 94、`(:TrackCircuit)-[r:NEXT_TC{方向:'下り'}]->()` 546、`(:TrackCircuit)-[r:HAS_BOUNDARY_SIGNAL]->(信号)` **312**(r.側='上り側'名古屋端/'下り側'亀山端、判定=信号キロvsTCキロ近似・不能時読み順。側不明0)。vision が図上で境界に信号を認めた物理境界のみ。
- NEXT_TC のうち22エッジに `要確認=true`(理由=キロ逆行 or 並列線線形化)。駅構内の並列線(下り本線/中線/上り本線)をvisionが直線鎖に線形化する誤読(TC名重複6図: 伊勢田/楽平/川越/網勘/浜元/六呂見)。隣接を辿る問いは `WHERE r.要確認 IS NULL` で除外(app.pyヒント明記)。コミットd0532df。
- **名前ベース被覆改善は撤回(875fbf1)**: 815d122でTC名↔信号(12RT↔12R等)を境界信号として補完(→456)したが、幾何検証(x↔キロ写像+TCラベルx)で測定可31リンク中23(74%)が非隣接と判明(西蔵12RT↔12R=2TC間、名古屋街道=9TC間)。TC名は進路の制御関係で物理境界と限らない。キロアンカーも遠方TCへ誤キロを付与しNEXT_TC逆行検出を汚染。→ vision由来312へ復帰。**西蔵の手前TC12RTが空なのは正しい**(12RTの物理境界は制御子V/Z、主信号でない)。支障報知停止信号12R/13Rは HAZARD_STOPS で回答済。教訓: 名称の論理関連≠物理境界。上部キロ見出しはテキスト抽出可28/94のみ。
- 境界信号は主信号パターン(数字+L/R、上り/下りN)で決定論フィルタ済(単独英字の制御子/中継U/S/C/W等を除外)。
- 併せてキロ導出の即時境界 `(:Crossing)-[:TC_BOUNDARY_SIGNAL{方向}]->()` 154。
- ビルドは `kb_demo_v6/crossing_tc_topology.py` の `build_cypher()`(抽出=`extract()`)。

検証: 蟹江∈HH3T 上り側境界=下り1・下り側境界=上り3(002一致)、API側クエリ正答。西蔵∈HKT 上り手前TC=12RT(ただし12RTの境界信号は西蔵図に未捕捉で1ホップでは12R/13Rに届かない=データ被覆の限界)。008等の支障報知停止信号そのものは [[added-data-integration-2026-07]] の HAZARD_STOPS を使う(トポロジーは空間裏付け)。9790=338凍結維持、try1×208=68/208(±9ノイズ帯67-73内、回帰なし)。app.py スキーマヒント更新済。関連: [[crossing-diagram-jiso-extraction]] [[crossing-diagram-symbol-rules]] [[eval-improvement-progress]]。

2026-08-21追記: 既存抽出は本線単一列読みで複線上り線が未収録だった。全94図をvision3パス再認識し方向付き1桁TC(上りnT/下りnT)58+NEXT_TC25を加法投入(extract/ingest_tc_uplines.py・30図・両環境一致)。添字表記(間々の上り5₁T/5₂T=下付き数字)は原図目視→分割TC慣行(Ⅰ/Ⅱ)へ正規化し上り5ⅠT/5ⅡTで投入済(利用者承認・原図表記プロパティ保持・計60TC)。教訓: 下付き添字/カナはvisionがベタ読みする(佐屋61イT→614T)ため2桁以上の方向付きTC読みは目視必須。EC2コンテナ内でllm_providers実行可(ローカルAWS認証切れ時の代替。kb_demo_v6/llm_providers.pyはsymlink=実体はkb_demo/)。
