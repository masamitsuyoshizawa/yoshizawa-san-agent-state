---
name: eval-and-table-comprehension
description: 回答方式評価(try1-5)でC単独が上限と確定・次の注力=踏切制御表/連動表の表意味の完全構造化
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-07-28T11:52:24.254Z
---

回答方式評価(2026-07-28実施)と、それを受けた次の注力方針。

**評価結論(確定, 208問)**: try1(/interlocking/ask, C単独)= correct 69/208(33.2%, 過去C評価≈70-71と一致)。answerable=yes の79問では correct 75%/c+p 91%。5方式比較(層化30問)で C主導(try1/try5)が c+p 60-70%、A重心・federate は 13-17%、try4(A+B+C)は incorrect最多=**過剰統合の害**。try5改良(try5b: C優先統合 / try5c: Cパススルー+A/B fallback)でも **208問で try1 超え不可**(try5c は correct 69=69 同点だが incorrect 17→28 悪化=fallbackが「情報なし」を創作化)。**結論: C単独が精度上限。「情報源を足すほど良い」は不成立**。精度向上余地は方式統合ではなく**Cグラフ自体の収録範囲・抽出精度**にある。モジュール備忘=`docs/eval/評価モジュール概要_20260728.md`、報告=`docs/eval/API回答方式比較_try1-5_層化30問_20260728.md`+artifact。

**次の注力(利用者方針 2026-07-28)**: Cグラフの抽出内容精度向上・不足データの追加グラフ化。**先ずは連動図表・踏切制御図表の「表」の部分を可能な限り100%理解**する。

**失敗分類(try1×208の非correct 139問)**: 凡例・記号の意味48 / 設置理由(なぜ)35 / 隣接駅記号解決21 / **表・図の具体値(鳴動条件/対向数/諸元等)10** / 標識警報機型式8 / 故障論理4 / 枝番転てつ器4。表理解の直接改善対象は「具体値」系10問(凡例・設計理由は図注記/設計知識で別課題)。

**実証したギャップ(確度: 確定)**: データはKGに在るが**表の意味が未構造化**。例=白髭踏切の`SoundingRule`は`鳴動条件`/`終止条件`/`進路`/`距離`/`時分`を**テキストで保持するのみ**で、鳴動条件を軌道回路ノードへの`TRIGGERED_BY`・論理(AND/OR)・但し書きに分解していない→「鳴動条件の軌道回路名」がhonest_gap/partial化。SoundingRreに**2系統のスキーマ混在**(関西本線=素テキスト / 別系統=`開始条件JSON`/`終止条件JSON`構造化済)。**関西本線の踏切制御表が未構造化**。これは [[reminder-crossing-control-table-rules]] の未完事項と一致。

関連 [[v9-api-schema-unification]] [[abc-unified-api]] [[added-data-integration-2026-07]] [[reminder-crossing-control-table-rules]]。
