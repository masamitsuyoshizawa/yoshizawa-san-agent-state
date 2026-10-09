---
name: c-diagram-kg-design-principles
description: C図表(連動図表・踏切制御図表)ナレッジ化の設計制約8点。新路線追加時に必ず遵守。
metadata: 
  node_type: memory
  type: project
  originSessionId: c1a2be54-8d9f-4694-bb17-39fcbe44075e
---

C-KG(連動図表・踏切制御図表→9990)を新路線対応・拡張する際の遵守制約(利用者指示 2026-07-08)。詳細計画は `/home/masam/.claude/plans/stateless-noodling-stream.md`、進捗は `docs/作業現状_20260708_追加データ統合.md`。

**Why**: 当初は飯田線2駅前提の作りで、関西本線(連動16駅+踏切94)追加・今後さらに増加のため、ハードコード排除・データ駆動・出典/PII厳格化・段階運用が必須。

**How to apply**:
1. 表は決定論(pdfplumber罫線・○囲い復元)で100%正確に読取→その後グラフ化。
2. 図は全抽出でなく必要情報を選択抽出。
3. 出典=ファイル名+バーコード下 `New:xxxxxxx`+最終更新日(右下表の変更年月日/印鑑日付)をノード・エッジに付与。踏切制御図表の履歴欄は氏名多数のため、グラフ保有は「ファイル名・最終変更日・New:コード」のみ(**氏名不保持**)。
4. 図/表のページ配置は可変。「表は常に2ページ目」等の固定禁止→動的判定。
5. 抽出パイプラインを更新対応可能なユーザUI(V8 tab_c 系)から実行可能に。
6. ノードIDに所属駅名・踏切名を含める(路線・駅・踏切で一意)。
7. 2駅前提・ハードコード(`ST`/`CROSSING_STATION`/`SIGNAL_KP_RANGE`/`CTRL_PREFIX`/`MANUAL_SUPP` 等)を排除し、路線別データ+9990照会へ。踏切→駅は9990グラフから導出。
8. 新記号・未検証ルールは人手検証を挟み段階的に。

投入先は 9990 のみ(9790は凍結・不変)。関連: [[added-data-integration-2026-07]] [[v8-cross-source-federation]]。
