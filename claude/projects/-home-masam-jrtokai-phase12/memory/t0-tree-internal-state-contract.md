---
name: t0-tree-internal-state-contract
description: 探索木の内部状態(決165〜170・dkb): I1 実装済み(dkg_tree_state.py 61f4b384・backend cde88d22・計画 改訂 5)・既存応答 1,636 件 byte 同一・E1 2,988/2,988・EC2 は別承認
metadata:
  type: project
---

2026-10-01 時点。決163〜166(T0 探索木を質問に応じて描く)の dkb 担当 P1 = 内部状態の契約の計画書 `docs/計画_探索木_内部状態の契約_dkb_20261001.md` 改訂 1(sha16 d8259fa31327c235・コミット bcd5b877)。fed は P2(口・Mermaid・Dify)で、鍵と欄の名は info 2316/2320/2326 で揃えた(候補 key = cid・質問 q:+Question.id・測定 m:+id・併発確認 qt:+文 sha16・list_no・answers[]・flag 5 値で promoted を追加)。

**Why:** 計画の形を決めた確定の事実 — 安全区分の印は語の照合で、質問 1,162/1,266(91.8%)・測定 1,633/1,716(95.2%)が未確認。所見があると答えは順位を変えず点数だけ変わる。候補を消す経路は無い(excluded は第 1 段で出さない)。2 段対偶は B:500-518。木に出る質問 292・測定 168(LOO 647 実測・measure_tree_e10.py)。

**How to apply:** 決166 で B1 = (ii) 見通しは描き停止枠 + (iii) 専門家が区分を付ける(台帳 dkg_safety_class_ledger.csv 案)。未決 B7(A2 は描かない)・B8(台帳)・B9(未確認のコスト)。次は fed P2 と揃えて rev P3 → 利用者 → 実装。実装時は `_diagnose_core` の切り出しで既存応答 byte 同一(E2)を先に確かめる。関連 [[verify-what-the-target-actually-reads]]・[[t0-tree-image-demo-plan]]。

**改訂 2(2026-10-02・決167・rev R165-1〜4):** sha16 fb5d8ae4c8b20b62・コミット 8ab19bd1。LOO 647 × 木 1 本の実測(measure_tree_r165.py)で、仮想の答えにより B 追補(bsim:)が消える事故 11.3%・足される 8.5% → 改訂 1 の「候補は消えない」「所見があれば順位不変」は誤り(B:3240-3242 で再ソート)。E10 最終集合 = 質問 257・測定 77(全未確認)。木 1 本 warm p95 1.46 秒・cold 13.1 秒。B7 承認・B8 条件つき・B9 = 確認済み優先の階層。未決 B10(消失は枝単位 STOP_INTERNAL)・B11(既存 B 伝播の「異常」「不明」→neg は第 1 段で直さない)。fed P2 改訂 1 2c83481e と欄の名を揃え済み。教訓: 推定で書いた不変条件(「消えない」)は、計画の段でも安く測れるなら測る。rev の静的指摘を実測で裏付けると議論が速い。

**改訂 3(2026-10-02・決168・rev R167):** sha16 43e2a46a5c155aa9・コミット 60cc5258。stops = {code, scope root|item|branch, at, on_value, reason, keys_kind?}(fed P2 改訂 3 00220f00 と同じ)。aggregate の安全 = 既知 A2〜A4 → 未確認 1 つで未確認 → 全承認 A0/A1 だけ確認済み → 判定不能は見通しも止める。台帳は承認 manifest dkg_safety_class_approvals.json の行内容 digest に照合(自己申告 hash と区別)。E1 比較は正規形/既知別名/不明に分ける。再測定: 枝の停止率 189/2,994 = 6.3%(11.3% は事故率)・文→id 2+ 15・0 件 0・aggregate 選択 3。B10(R167-1 条件)・B11 承認。教訓: 率は分母の単位(事故・枝・鍵)を併記する(rev に指摘された)。

**I1 実装(2026-10-02・決169/170):** main 67ea553d。dkg_backend.py cde88d22b2a0e068(diagnose → _diagnose_core → _diagnose_impl・13 箇所に ContextVar 収集器 _WHY・追加 67 行のみ)・新規 kg_api/sources/dkg_tree_state.py 61f4b384ba82dac7・台帳(見出しだけ)と承認 manifest(決170・行 0・rows_digest = sha256("[]"))。E2: diag_snapshot.py で 4 群 1,636 件 byte 同一。E1(a)647/647・E1(c)2,988/2,988・公開上位 5 一致・E5 100/100・E7 warm p95 1.57 秒・cold 10 秒台は予算切れ。試験 単体 30・実機 8。残: fed I2(warmup の置き場・口)・bkb 受入・EC2(別承認)・KG_DKG_GRAPH_DIGEST の値の渡し方。教訓: 応答の dict に欄を足すと漏れる → 収集器は候補 dict の外(ContextVar)に置いた。
