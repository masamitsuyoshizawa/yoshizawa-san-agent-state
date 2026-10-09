---
name: ingest-pipeline-enforcement
description: B/D-KG の投入は ingest_pipeline.py --mode commit のみ(積荷は accident/propagation/normalize)・接続は kb_conn(環境変数必須・rehearse で正本拒否)・演習は rehearse_env.py で合否は終了コードでなく rehearse_result.json・書き出しは演習前後に取り直す・段1正規化は2026-09-17にB投入済
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-17T00:23:21.529Z
---

2026-09-11〜12(dec 20260911-1930 / 20260912-0010): X16 手順 6「適用の担保」で、B(9890)/ D-KG(10090)の新規データ投入は
`accident_kb_v7/scripts/ingest_pipeline.py --mode commit`(段ゲート G0〜G11・承認ファイルは dec 必須)が**唯一の経路**。
`ingest_one.py` の直接実行・個別段スクリプトの手実行は使用禁止(X13 `direct_ingest` で検出)。
接続は `kg_api/kb/scripts/kb_conn.py` のみ(URI・パスワードは環境変数必須・既定値なし。`KB_CONN_MODE=rehearse` は正本 URI を拒否、`dry` は接続拒否)。
ローカル実行は `KB_ACCIDENT_NEO4J_URI` / `KG_DKG_URI` / `KG_NEO4J_PW`(値は printしない)の設定が要る。
統制 config 22 件は `accident_kb_v7/config/pipeline_manifest.json` に sha256 登録(変更は dec → `--register-manifest --dec`)。
commit の開始時ダイジェスト台帳は `knowledge_kb_v8/data/eval/graph_registry.json`(git 外)。
隔離演習は `rehearse_env.py --pdf … --category …`(一時 Neo4j 2 基へ export 復元→復元ダイジェスト一致確認→演習→正本不変記録。約 7 分・LLM 呼出あり=saml2aws ログイン要)。四半期+新種データ受入時に実施。
最終演習 2026-09-12 合格(手順のみ 0 件・記録 rehearsals/20260911-1006)。

**Why:** 2026-09-11 に演習用スクリプトの「既定値=正本」接続と DETACH DELETE で実 B が書き換わる事故があり、dec 1810 で復元、dec 1930 で再発防止と段ゲート化を決定。
**How to apply:** B/D-KG へ書くコードは kb_conn 経由・X13 PASS 必須・`bolt://` リテラルや DELETE を書かない。新規データは pipeline の commit モードだけ。演習は必ず rehearse_env 経由。関連: [[dkb-recurrence-improvement]] [[session-comms-worktree]]

**積荷は 3 種**: `--payload accident`(G0〜G11・PDF 1 件)/ `propagation`(G0→G12)/ **`normalize`(G0→G13・req 20260918-0000・利用者承認)**。
`normalize` は既存ノードへの**属性の加法**で、投入器は `accident_kb_v7/scripts/normalize_text_fields.py`(`select`/`checks`/`ingest`)、固定入力テストは G0 の `TESTS` に登録する。
**2026-09-17 に B へ投入済み**: `Accident.summary_norm` 695・`FailureMode.text_norm` 1,767(値が変わるのは 956)・`norm_source` 付き。値は `NFKC(原文)` のみで**原文は不変**(投入の前後で sha256 を照合し、違えば中止)。台帳の `b` は `11411d8ce9a06646`。**戻すのは `REMOVE n.summary_norm, n.text_norm, n.norm_source` だけ**(ノード・関係・原文は不変)。**半角の語で `_norm` を引くと 0 件**になる(半角が全角へ寄るため)ので、照会は全角で引く。`place_text` は入れていない(変化 402 件の主因が全角波ダッシュで検索に効かないため)。

**演習(`rehearse_env.py`)の落とし穴 2 つ**(2026-09-17):
- **合否を終了コードで判断しない。** 復元ダイジェスト不一致で**中止**したのに、背景実行の集約では**終了コード 0** と表示された。判定は**出力の最終行**(「不変=True」と「記録 →」)か **`rehearse_result.json` の `pipeline_rc == 0` かつ `canonical_unchanged == true`**。
- **書き出しの名前の日時は「取った時刻」で、「どの便の後か」を表さない。** `dkg_20260914-2222` は同日の影響関連 第 2 便の**投入前**に取ったもので、復元が正本と一致せず演習が中止した。`.meta.json` にダイジェストは入っていないので、**演習の前に両系の書き出しを取り直す**のが確実(読み取りのみ)。**投入後も取り直しておく。**

**共有台帳 `knowledge_kb_v8/data/eval/graph_registry.json` は B/D 系(`ingest_pipeline.py`)と C 系(`ingest_diagrams.py`)が共有する。**
2026-09-14 22:26:39、dkb が実行した影響関連 第 2 便の commit で、`ingest_pipeline.py` が台帳を丸ごと上書きし C 系のキー(`c`・`c_ec2`・`c_eval_baseline`・`c_eval_baseline_history`)を消した(ckb が検出・req 20260915-1005)。
2026-09-15 に `write_registry()` で read-modify-write へ是正(2f35f1b)。所有キーは `b`・`dkg`・`ts`・`stem`・`dec`・`git_head` だけで、**他系統のキーが失われるなら書かずに停止**する。テスト `test_registry_write.py`。
**共有ファイルに書く処理を足すときは、読んでから自分のキーだけ差し替え、他系統のキーの消失を検査する。** 検査(G0)が台帳の一致を見ていても、書く側が他系統を保持するかは別に確かめないと漏れる。
