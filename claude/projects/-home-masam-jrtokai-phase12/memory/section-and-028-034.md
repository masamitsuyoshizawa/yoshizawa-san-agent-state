---
name: section-and-028-034
description: 駅間(区間)付与でinterlocking-028/034を正答化(信号てこ差・現示段数の一般知識をglossary化)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-02T15:15:18.742Z
---

interlocking-028/034(着手保留分)を正答化した(2026-08-03、コミットf342e4a)。

**interlocking-034**(四日市～南四日市の上り1/下り1 全現示と差異): データは既に正しかった(上り1=5現示 進行・減速・注意・警戒・停止、下り1=3現示 進行・注意・停止)が、**駅間で絞れない**のが障害だった。`scripts/assign_section.py` で各踏切のキロを挟む隣接駅から駅間名 'A～B' を決定論算出し `Crossing.区間` / `BlockSignal.区間` に加法SET(9990/EC2、200件)。四日市=37140m/南四日市=40400m の区間に第二鹿化/第一天白があり両者上り1=5・下り1=3。app.py に区間フィルタ+現示段数差の理由ヒスト、glossaryに『閉そく信号機の現示段数の違い』(先方場内への近接で警戒/接近先の警戒で減速が付加)。

**interlocking-028**(河原田 鎖錠転てつ器なし信号=イ・ロ、閉そくとの違い): パート1(イ・ロ抽出)は既に正答。パート2の違いを、app.py SCHEMA_HINTS + glossary『場内・出発信号機と閉そく信号機の違い(信号てこ)』で正答化: 場内/出発(イ・ロ含む)は信号てこで扱い者が停止/進行を制御可、閉そく信号機は軌道回路で自動、が本質(転てつ器鎖錠の有無でない)。

**重要な設計知見**: `SCHEMA_HINTS` は cypher生成にのみ渡り、`format_answer`(回答合成)には届かない。回答生成には強い『なぜ/理由は注記が無ければ説明禁止』ガード(ハルシネーション抑止)がある。よって概念的説明を回答させるには **glossary.json に出典付き用語を追加**する(glossary_lookupがgctx経由でformat_answerに届く)のが正解。aliasは過剰発火を避け特異的に。関連: [[eval-and-table-comprehension]] [[added-data-integration-2026-07]] [[crossing-tc-topology]]。
