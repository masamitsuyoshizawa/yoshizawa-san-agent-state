---
name: safety-mark-display
description: 安全区分の印(safety_mark)の表示層対応(2026-09-22・fed)— Dify の文面はプラグインが組み立てる・印は別行・印なしは不変
metadata:
  type: project
---

coord req 20260916-1500(利用者判断 A-2)で、API の `safety_mark`(dkb 実装・sm1-20260916)を表示層へ出した。fed コミット 9e25947・EC2 反映は coord へ req 20260922-0900。

- **Dify の D-KG 表示は DSL ではなくプラグイン `deploy/dify/plugin/jrtokai_kb/tools/dkg_diagnose.py` の `_format` が組み立てる**。DSL は `{{#tool.text#}}` を流すだけ。改修は version 上げ(0.3.5→0.3.6)→パッケージ→upload/install→DSL の `plugin_unique_identifier` 追随(coord)。
- **印は質問の行とは別の行**に置く。Dify の状態整理 LLM は「質問原文を括弧まで一字一句」拾って API に戻すので、同じ行に足すと既出質問の照合が外れうる。プロンプト変更は利用者承認事項。ローカル Sonnet 4.6 で取り込み 0 を確認(`smoke_dify_state_llm.py`)、本番 Sonnet 5 は反映前スモーク。
- class が無い項目・欄の無い応答には何も足さない(出力が変更前と完全一致を `test_safety_mark_display.py` で機械確認)。印が出たときだけ「印の無い項目も安全を意味しません」を 1 回。区分名は API の label をそのまま。
- 方法欄の本文は API が返さない(`_method` 除去)ので、検出語と欄名+出典を出す。
- OWNERS に `deploy/dify/dsl/*`・`deploy/dify/plugin/*` を fed 所有で登録。
- dkb が実応答 5 通り(A4/A3/A2/null/欄なし)で K1〜K7 確認・指摘なし(128 検査 PASS)。EC2 反映は利用者へ諮る(coord)。反映時は稼働版と正本を先に diff。
- 指令 UI `kg_api/demo_shirei/*` は EC2 稼働中(保全 split-20260908)で **fed 所有**(coord req 0950・fed rep 1000 で異論なし)。**着手は A-2 の EC2 反映後**。描画 4 か所(action_row・対話の次の質問・対話と判定書の**コピー用メモ=平文**)。見積 0.5〜1 人日。**coord 裁量で承認済み(OWNERS 登録 06777e4)**: コピー用メモは印のある行だけ行末に短い印+メモ内に注記 1 行、**印の無いメモは変更前と完全一致を機械確認**。demo_v10 には平文の出口なし(coord 確認)。
- **未達の課題**: API が方法欄を `_method` pop で捨てるため、ms:0096 の「取り外して」の本文は依然見えない(coord req 0940 で dkb が見積中)。方法欄は非空 1,671・中央値 35 字・最大 258 字・改行 0。出すなら表とメモには入れず詳細/別行に全文+「方法欄(原文・専門確認前)」の見出しと出典。

**Why:** 取り外し等を伴う検査が現場の画面で区別できないまま出ていた(ms:0096 の件)。
**How to apply:** Dify の表示を変えるときはまずプラグインを見る。表示に文字を足すときは、LLM が拾う行と混ぜない。関連: [[fault-tree-stage3-fed]] [[dify-demo-ec2]]

**方法欄の本文の表示(2026-09-22・dec 20260922-1300 利用者承認・API は dkb 90d93f1)**: `DKB_RETURN_METHOD=1` で `next_measurements` に `method`(全文)と `method_note`(state「原文・専門確認前。保全標準の定期検査の方法であり、障害時の指示ではない」・check_type・refers_elsewhere)が対で付く。表示は fed: **Dify プラグイン 0.3.7**(測定の行と印の行の後に「方法欄 — state(検査種別・出典)」+本文+参照注記)・**demo_v10 の詳細欄**(警告の後に caption+引用。基準欄と同文なら本文を繰り返さない)。**一覧表・コピー用メモ・`_dkg_digest`・demo_shirei(actions に本文なし)には出さない**。欄が無ければ出力は変更前と完全一致(`test_method_display.py`・基準 1be21fc)。状態整理 LLM は方法欄の行があっても 8/8。

**A-2 の EC2 反映完了(coord info 20260922-1800)**: 本番 Sonnet 5 でも印は質問原文に混ざらない。**この Dify 版にはローカル .difypkg で既存プラグインを更新する API が無く uninstall → install が要る**(資格情報は消えず・DSL は自動で新版へ解決)。

**DSL の正本(fed 所有)**: 稼働版と比べると dependencies から bedrock 0.0.79 が落ちていた(行 diff は Dify の正規化に埋もれて見えない)。**fed の判断=稼働版のエクスポートに揃える**(rep 20260922-1900・置く作業は coord・秘密情報の確認と前後の意味比較が条件)。意味比較は `knowledge_kb_v8/scripts/fed/dify_dsl_semantic_diff.py`(描画値を捨て、依存・プロンプトとコードの sha256・ノードと辺・ツール設定を比べる)。

**指令 UI の印(req 0950)を実装**: `kg_api/demo_shirei/app.py` に demo_v10 と同文の `sm_*`。action_row・次の質問・判定書の一覧に警告、**コピー用メモは印のある行だけ行末に短い印+メモ内に注記**。印の無い応答は変更前と完全一致(`kg_api/demo_shirei/test_ui_safety_mark.py`・61 検査)。EC2 反映は req 20260922-1910。

**方法欄の本文の EC2 反映完了(coord info 20260922-2000)**: プラグイン 0.3.7・`DKB_RETURN_METHOD=1`・M1〜M5 合格(本番 Sonnet 5 も方法欄の行を質問に取り込まない)。req 1500 の元の課題(ms:0096 の取り外しの本文が見えない)は解決。**docker cp でファイルを入れた後にコンテナを再作成すると失われる**(先に docker commit で :latest を更新してから compose up -d --no-build)。DSL 正本のうち auto_routing・ask・federate_lite の 3 本はまだ 0.3.5 参照で、稼働版エクスポート(EC2 ~/dsl_backup_20260922/)への置き換えは coord が行う(fed は意味比較の記録を確認)。

**一覧の外にある印(coord 判断 (b)・fed a08c679)**: 指令 UI で先頭 N 件に切る一覧の外に印のある確認・測定があるときだけ「この一覧は先頭 N 件です。N+1 件目以降に…M 件あります(印の無い項目も安全を意味しません)」を 1 行(件数・順序は不変)。判定書(5 件)に加え、fed の判断で聞き取りナビのメモ(4 件)とワンボード予備(7 件)にも適用し coord に明記。**実応答では安全措置・通知・機材が先に並ぶので、先頭数件に印が入らないことが多い**。

**一連の完了(coord info 20260922-2100)**: DSL 正本 5 本を稼働版エクスポートに置換(秘密情報の検査は検出なし・意味比較 5 本とも一致・識別子は全 0.3.7)。指令 UI も EC2 反映済み(保全 pre-smark-20260922・範囲を広げた分も coord 承認)。**印が出る画面は demo_v10・federate・Dify の D-KG 診断・指令 UI の 4 つすべて**。意味比較スクリプトは tool_parameters の `{"type":"mixed","value":x}` も畳むよう修正(包み方だけの差が出ていた・variable は畳まない)。rev 第 7 段(req 20260922-2120)へ読み方表と T0 シートを揃えて提出済み。
