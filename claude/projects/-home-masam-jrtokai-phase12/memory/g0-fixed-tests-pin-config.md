---
name: g0-fixed-tests-pin-config
description: 工程の G0 は固定入力の試験を回すので、設定の値を集合の一致で決め打ちする試験があると、正しい加法でも全部のジョブが G0 で止まる
metadata:
  type: feedback
---

`ingest_pipeline` の G0 は、固定入力の試験(`test_registry_write.py` など)を毎回回す。その中に kb_conn の `CANONICAL` を「ちょうどこの集合」と照らす行があり、決344 で coord がコンテナの名を加法した枝だけを入れると、試験が 62/63 で落ち、演習も commit も全部のジョブが `PIPELINE_FAILED` になった(2026-10-31・演習の台で再現)。試験の意図は「加法だけ」なので、包含(`>=`)で照らすよう dkb が直した(f7bbf0a39)。

**Why:** 共有の設定を変える側(coord)の試験は全部通っていたが、別の道具の固定入力の試験が同じ値を決め打ちしていた。受入で台を通さなければ、main へ入れた後に初めて全ジョブが止まって見つかった。

**How to apply:** 共有の設定(kb_conn・辞書・manifest など)の変更を受入するときは、G0 の固定入力の試験の一覧を回すか、台で 1 件通す。「加法だけ」を照らす試験は集合の一致でなく包含で書く。直した試験は設定の変更より先に main へ入れ、rep に反映の順の依存を書く。関連: [[deploy-dependency-in-rep]] / [[check-what-the-target-mechanism-scans]] / [[gate-allowlist-not-denylist]]

追記(2026-11-01 00:25): 同じ試験の 215 行目の鍵の集合(`set(KC.CANONICAL) == {...}`)も完全一致のまま残っていた(tkb が気づいた)。暗黙知 KB の tacit を kb_conn に足すと落ちる。dkb の枝のコミット 0187b4953 で包含に直した(今の kb_conn と tacit 入りで 63/63・直す前は 62/63)。coord が計画を改め、先に main へ入れた(決342 追補 11・2026-11-01 00:26・本体ツリーの試験 3cb20614a98fda47)。dkb の枝の早送りの止めは解けた。kb_conn の tacit は rev の段 1 の点検の後に入る。直すときは、値だけでなく鍵の集合も見る。
