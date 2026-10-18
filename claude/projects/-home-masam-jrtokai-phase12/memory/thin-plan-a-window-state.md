---
name: thin-plan-a-window-state
description: 決263-A 薄い窓口(fed)の状態・通しの受入 84/0(2026-10-17)・通しを回すときの接続の環境変数の渡し方
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-16T17:02:12.832Z
---

決263-A の fed 分(承認窓口 `knowledge_kb_v8/scripts/fed/thin_window/`: ledger・core・app・launch・e2e_thin_window)は、実装と通しの受入まで完了(rep 20261017-0200・合格 84/0・コミット fbfdb9b3・main 161a91ed)。設計書 改訂 12(db9a804f78f665b8)。名簿 operators.json 初版 e3b74e9d48fb2ff1(masam@DESKTOP-OCT2024・G2/G3_SPAN/G3_BUNDLE/G4/G5)。coord の決263-A 追補 9 に記録済み。次は rev の実装確認 → 決264(LLM を呼ぶ演習)。rev rep 0309 の R263A-2〜5 は是正済み(rep 20261017-1107・main e0d91076・単体 66・画面 AppTest 32・通し 84・画面からの架空の氏名の通し 11)・R263A-4a も是正(rep 20261017-1205・17/0)。**決264 の LLM 隔離演習は 2026-10-18 に利用者が実ブラウザで完了**(演習 20261018-185646-1757e8a0・rc 0・正本不変・台帳 seq 45 93dee2cc1e88bc95・承認 5・G8 は決265・費用 0.142315 USD)。fed の読み合わせ = 不整合なし(rep 20261018-1930・G2 候補 0 件で漏れ照合は対象 0・G4 の「双子でない」は別名の写しなので判定結果として読まない)。窓口側の手順書 docs/手順_LLM隔離演習_窓口側_fed_20261017.md(venv の Python で起動)。

**Why:** 再開時に、どこまで済んで何が待ちかを取り違えないため。

**How to apply:** 変異で検査を確かめるときは、守りが 2 重だと片方を外しても同じ理由コードで止まり見逃す(R263A で 3 種)→ 例外の型・1 か所化・片方だけが効く入力で区別する。同じ PDF の演習は stem と F の名が同じなので、台帳の照合は必ず run_id で絞る。 通しの受入(e2e_thin_window.py)を回すときは、正本の接続先を環境変数で渡す(既定値なし・未設定だと rehearse_env の開始が KbConnError で止まり、台帳に終了なしの session_start 行が残る)。`KB_ACCIDENT_NEO4J_URI=bolt://localhost:9890`・`KG_DKG_URI=bolt://localhost:10090`・パスワード(`KB_ACCIDENT_NEO4J_PASSWORD`・`KG_DKG_PW`)は `docker inspect neo4j-accident-v7 / neo4j-dkg-v1` の NEO4J_AUTH から表示せずに渡す。演習はホストに 1 件(予約)なので LOCKS と info で予告してから。窓口の状態の置き場は一時ツリーの `accident_kb_v7/data/pipeline_state`・演習の記録の欄は `tree`。関連 [[table-row-order-is-not-check-order]]・[[stop-point-3-impl-pending]]
