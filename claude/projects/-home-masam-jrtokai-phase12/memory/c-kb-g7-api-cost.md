---
name: c-kb-g7-api-cost
description: C-KB の G7(208問評価)1回の実費はキャッシュ導入前73.6ドル・導入後22.54ドル。「生成≈7ドル」は誤り
metadata:
  type: project
---

図表KB(C-KG)の G7 208 問評価 1 回の API 実費は、プロンプトキャッシュ導入前が **約 73.6 ドル**(CloudWatch 実測・11 区間平均。生成 67.6)、導入後が **22.54 ドル**(生成 15.80+判定 6.74・2026-09-18 実測)。
comms で長く使われた「生成 ≈7 ドル/208 問」は ckb の根拠のない外挿であり 2026-09-18 に撤回した(rep `docs/comms/20260918-1420_ckb_to_coord_rep_costdown-plan.md`)。

- 生成の入力の約 92% が全問共通の固定部(nl_to_cypher のスキーマ+ヒント 27,400、format_answer の system 14,300、enrich_lookup 27,400)。
- 回答側の cachePoint 導入(A0〜A6/A8・dec 20260918-1600)で生成の送信の 91.2% がキャッシュ読みになった。送信量は不変(1 問 54,226)、所要時間は短縮しない(支配要因は回答整形の出力生成)。
- 実呼び出しは 1 回の G7 で 474 回(旧ハードコード 416 は 14% 過小)。enrich_lookup の発火率は 40%(従来前提の 25% ではない)。
- 記録上 G7 の生成は 11 回(`knowledge_kb_v8/data/pipeline_state_c/*.eval`)= 累計で約 583 ドルの過小報告。

関連: [[eval-improvement-progress]] [[estimates-not-labeled-as-measurements]]
