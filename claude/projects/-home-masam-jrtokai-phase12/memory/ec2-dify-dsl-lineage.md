---
name: ec2-dify-dsl-lineage
description: EC2 の Dify D/G の DSL は main の正本と別系統(決251 の変数なし)・EC2 を触るときは EC2 用の版を元に最小差分・console の公開前の検査は API の publish より厳しい(2026-10-21 決285)
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-21T02:24:17.542Z
---

EC2 の Dify の D/G(原因探索の対話 2 本)の DSL は、main の正本 `deploy/dify/dsl/dkg_{diagnose,guided}_chatflow.yml` と**別系統**。EC2 は fed 7f122884 の正本(diagnose 701792b0318a94b2 / guided c648b8af3bc5ee6d・決227)に、決285 で finish_reason の参照を外した版(3e553d7ead47a030 / 55c8dd6466c340b6・置き場 knowledge_kb_v8/data/dify/finish_reason_20261021/)。main の正本は決251 の表示契約の変数(last_intake_json ほか)を含み、EC2 には入っていない。

**Why:** main の正本をそのまま import すると、承認していない決251 の変数まで EC2 に入る。2026-10-21 の是正では、EC2 の現行に文字の上で 2 か所だけ当てる道具(knowledge_kb_v8/scripts/fed/dify_drop_finish_reason.py)で最小差分を作った。

**How to apply:** EC2 の DSL を直す依頼では、main の正本と EC2 用の版の両方を直し、EC2 用は EC2 の現行(coord の export か上の版)に最小差分で当てる。正本の生成は dify_plan_a.py → dify_display_contract.py(決251)の連なりで、後段が前段の文字列(main の引数の行など)を探して置き換えるので、前段だけ変えると後段が当たらない。Dify の console の公開前の検査は、LLM の節点の出力に無い欄(finish_reason)への参照を「無効な変数」として公開を止める(API の publish は通る)= 参照の検査を試験(test_dify_plan_a.py)に入れてある。関連 [[dify-plugin-update]] [[dify-demo-ec2]]

**決290〜292(2026-10-22)ボタン回答の層**: EC2 系統の D に human-input の段を 3 つ足す生成の道具 `knowledge_kb_v8/scripts/fed/dify_button_layer.py`(ブランチ fed-btn・出力 data/dify/button_20261022/・層なしは基準と byte 同一 = yaml.safe_dump の sort_keys=False・width=10**9 で往復する)。Dify 1.16.1 の事実(原文の抜粋 = docs/抜粋_探索木_ボタン回答_Dify原文と結線_fed_20261022.md): human-input はループに置けない・再開は止めた時点の変数プールを使い会話変数を DB から読み直さない(会話変数の札では古いカードを守れない → サービス API で読み直す G1・照らしの直後に 1 つの assigner で状態と番号表)・画面は本文の {{#$output.…#}} を入力欄にする。プラグイン 0.3.18(fed-p318)は機序の行の [:120] が出典の「]」を落としていた是正。test_fed_profile の PD は「固定した写し + LATER_DSL_CHANGES = 今」— DSL を承認で変えたら一覧に足す。
- 2026-10-23 決293: EC2 の **D はボタン回答版(38d8fe2aeed5675e・決285 の版に層を足したもの)**、G は決285 の版(55c8dd64)。main の正本(決251 入り)との系統の統合は別件のまま。

**決296(2026-10-23)本番反映済み**: 本番 D =「別の質問にする」版 DSL 08aeae3839312d50(105 節点・human-input 18・決295 = わからないを廃止し同じ図の次の未回答の質問へ差し替え・上限 2)。退避 backup_D_pre_t296_20261023.yml(e8150eae61b4bec0)。D を直すときの基準はこの版。
- **決308(2026-10-24)**: Dify の `MAX_TREE_DEPTH` を 50 → 200(/home/ubuntu/dify/.env・web と api を `docker compose up -d web api` で作り直し・停止 約 2 分)。D のボタン回答版は最長の一本道 71 節点で、console の公開前チェック「1ブランチあたりの最大ノード数: 50」に掛かっていた(API 経由の publish は掛からない)。**console を開いただけで draft が自動保存され、http-request 節点に retry_config(3 回・100ms)が入る・位置が変わる** → draft ≠ 公開版になる(info 1429・fed 判断)。human-input の delivery は webapp だけなので console のプレビューではボタンが実行の再開に結びつかず段 1 が出直し、2 回目は 412(決307 追補 7)。
- **決309/310(2026-10-24 15:20)**: D の DSL = **355d3e91dd4f5602**(44bac403 + http-request 3 節点〔35109/35209/35309〕に retry_config {retry_enabled: false, max_retries: 3, retry_interval: 100} を明示)・0.3.20 の識別子。理由 = Dify 1.16.1(graphon)の http-request は再試行(か error_strategy)が有効だと 2xx 以外を節点失敗にする(node.py 207 行)ので、有効にすると G1 の「status≠200 は SKIP で続行」が壊れる。console は null の欄を開くと既定値(有効)を自動保存するため、無効で明示して draft のずれを止めた(一時アプリで false は不変・null は true に変わることを確認)。退避 backup_D_pre_t310 e84b729e・_t307.yml 44bac403。
