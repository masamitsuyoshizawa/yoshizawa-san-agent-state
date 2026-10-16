---
name: thin-plan-a-window-state
description: 決263-A 薄い窓口(fed)の状態・通しの受入 84/0(2026-10-17)・通しを回すときの接続の環境変数の渡し方
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-16T17:02:12.832Z
---

決263-A の fed 分(承認窓口 `knowledge_kb_v8/scripts/fed/thin_window/`: ledger・core・app・launch・e2e_thin_window)は、実装と通しの受入まで完了(rep 20261017-0200・合格 84/0・コミット fbfdb9b3・main 161a91ed)。設計書 改訂 12(db9a804f78f665b8)。名簿 operators.json 初版 e3b74e9d48fb2ff1(masam@DESKTOP-OCT2024・G2/G3_SPAN/G3_BUNDLE/G4/G5)。coord の決263-A 追補 9 に記録済み。次は rev の実装確認 → 決264(LLM を呼ぶ演習)。限定 3 点(二重起動等は単体依存・氏名候補 0 件・G5/G6/G11 で止まらず)を決264 の前に埋めるかは rev に照会中。

**Why:** 再開時に、どこまで済んで何が待ちかを取り違えないため。

**How to apply:** 通しの受入(e2e_thin_window.py)を回すときは、正本の接続先を環境変数で渡す(既定値なし・未設定だと rehearse_env の開始が KbConnError で止まり、台帳に終了なしの session_start 行が残る)。`KB_ACCIDENT_NEO4J_URI=bolt://localhost:9890`・`KG_DKG_URI=bolt://localhost:10090`・パスワード(`KB_ACCIDENT_NEO4J_PASSWORD`・`KG_DKG_PW`)は `docker inspect neo4j-accident-v7 / neo4j-dkg-v1` の NEO4J_AUTH から表示せずに渡す。演習はホストに 1 件(予約)なので LOCKS と info で予告してから。窓口の状態の置き場は一時ツリーの `accident_kb_v7/data/pipeline_state`・演習の記録の欄は `tree`。関連 [[table-row-order-is-not-check-order]]・[[stop-point-3-impl-pending]]
