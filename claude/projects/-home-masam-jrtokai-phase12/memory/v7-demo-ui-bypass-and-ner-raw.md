---
name: v7-demo-ui-bypass-and-ner-raw
description: V7 デモの登録・辞書画面が正規の入口の外から B へ書けた → 決260 で無効化済み(19ec4847)・NER の原文送信は例外 (A) で承認(2026-10-16)
metadata:
  type: project
---

2026-10-16 の決259 の棚卸しと、fed の申し送りの確認で分かったこと(bkb rep `20261016-1255_bkb_to_coord_rep_fed-handover-1-3.md`・棚卸し `docs/棚卸し_次フェーズ構想_bkb_20261016.md` 改訂 1 853f08f3fa508264)。

- `accident_kb_v7/demo/accident_ingest_ui.py:93` は `ingest_one.commit` を直接呼ぶ。`run_v7.sh` は B の既定の接続先が 9890(正本)で、公開の旗も `KB_CONN_MODE` も無い。起動すれば、ingest_pipeline・G0・LOCKS の外から正本へ書ける。
- `accident_cause_dict_ui.py` は `KB_ACCIDENT_READONLY` を見ていない。公開の配備でも統制の設定 2 本を書き、`cause_concepts.py` で B へ MERGE する。`accident_dict_ui.py` は旗で止まる。
- X13 は `accident_kb_v7/scripts` だけを走査し、`demo/` と直下の `.sh` は範囲外。
- `detect_names.py` はマスク前の原文を Bedrock(`jp.anthropic.claude-opus-4-7`・ap-northeast-1)へ送る。設計確定メモ §6-3「マスクは LLM 投入前」と食い違い、承認した dec は見つからない。利用者の判断待ち(選択肢 A/B/C)。
- Opus 4.7 は `supports_temperature=False` で、温度 0 の指定は無視される。
- 新しい事故摘録の本投入は 0 件(演習では G0〜G11 を完走済み・2026-09-11)。演習 1 件で人手の承認が 5 回。
- 所管: `accident_kb_v7/demo` と `run_v7.sh` は OWNERS に登録が無い。scripts は dkb、kb_demo_v6 は ckb。


**決着(決260・2026-10-16 利用者承認)**: 所管 = bkb(OWNERS に `accident_kb_v7/demo/`・`run_v7.sh`)。bkb コミット 19ec4847 で次のとおり是正した。否定例 `test_v7_demo_disabled.py`(現行 4/4 合格・是正前の版 5741b339 では 4/4 が狙った理由で落ちる)。
- 登録画面・原因概念の辞書画面: 起動で止める
- 組織略称の辞書画面: 常に読み取り専用
- run_v7.sh: 資格情報の既定値を外し、未設定なら止める

EC2 の該当(streamlit-v7・neo4j-v7-akg)は停止中で、反映は不要だった。NER の原文送信は例外 (A) として承認され、設計確定メモ §6-3 の 3a の案文を rep 1310 で渡した(本体は coord が入れる・AWS 側の保持の根拠は coord の追補 1)。

**追補 2・3(2026-10-16)**: Bedrock の呼び出しの記録が有効(ap-northeast-1・text 配信・CloudWatch 無期限・S3 ライフサイクル無し)で、NER を回した手元の資格情報も同じアカウント(確定)。2026-06-05・07-08 の NER の原文は記録期間内。例外 (A) の条件「保持しない」を満たさず、利用者に再度諮り中。§6-3 の 3a の文言は、その結論の後に bkb が合わせる。

**Why:** Challenge A(顧客セルフサービスの登録 UI)の下敷きになる既存の画面が、規律(入口 1 本・統制語彙の承認)の外に在る。
**How to apply:** B の登録や辞書の UI の話が出たら、まずこの 2 画面の是正の状態と、NER の承認の決着を確かめる。「LLM 投入前にマスク」と書く前に、段 3 の例外を併記する。[[ingest-pipeline-enforcement]] [[claim-wider-than-implementation]]
