---
name: stop-point-3-impl-pending
description: 【2026-10-17 受入合格・bkb-a263 31eba4a9】次フェーズ A 薄い案の停止点 3(ingest_one の finish の分離・4 本の全件処理を当該 1 件へ)は bkb が実装・dkb が設計確認と受入(dkb rep 20261016-1442)・利用者承認待ち
metadata:
  type: project
---

2026-10-16、bkb の見積り(rep `20261016-1440_bkb_to_coord_rep_thin-plan-estimate-bkb.md`・A 3.5〜6.25 人日)に対し、dkb が coord に合意を返した。担い手は、実装 = bkb、設計の確認と受入 = dkb(rep `20261016-1442_dkb_to_coord_rep_thin-plan-ownership-b-scripts.md`)。所管(`accident_kb_v7/scripts`)と B 正本は dkb のまま。LOCKS は bkb が取り、dkb へ info を出す。**着手は利用者の承認(決262 の予定)の後。** coord も同じ条件を確認した(2026-10-16)。

dkb の受入の条件:
1. 実装の前に設計を dkb に見せる。
2. 開発と受入は演習の環境だけ(正本・EC2 は対象外)。
3. 全件処理と、当該 1 件に絞った処理とで、B の digest と stage2・census が既存 700 件の範囲で同じになることを写しで示す。
4. `apply_categories` の候補の段の DELETE → MERGE(94〜95 行)を、承認の後の段へ移すか、残す理由を書く。

設計の要点(rep 1440):
- `_commit_finish` を「候補の束(当該 1 件・グラフと census に書かない)」と「承認後の適用」に分ける。
- 束の sha を承認ファイルに書かせ、否定例 8 つを入れる(束の外のファイルの変化が混ざらない、を含む)。
- 採番と適用は排他のロックで 1 本に絞る。
- LLM の費用は CloudWatch の指標を coord が読む案が推し。呼び先の `invoke` は使用量を捨てている。

**設計(決262・2026-10-16)**: `docs/設計_薄い案_A_bkb_20261016.md` 改訂 0(c5c071593053cca8・f82ea622)を書き、dkb へ確認を依頼した(req 20261016-1452)。coord への rep は dkb の確認の後に出す。
- 要点 1: `apply_categories` のグラフの書き込みは、採番より前なので新しい 1 件には効かない。候補の段から外し、適用にも移さない推し。
- 要点 2: 演習の一時ツリーは `data/` が空で全件処理を確かめられない。同値性は、既存のデータを写した演習で、2 つの DB を比べて示す。
- 要点 3: 束の照合 9 項目と負例 N1〜N10。LLM の使用量は botocore の after-call で記録し、共有の llm_providers は変えない。単価表に Opus 4.7 の行は無い。
- 工数: 4.0〜7.25 人日。B の分は含めない推し(profiles/b.json は作らない)。
- 改訂 1(012c35c1b8cd46f0・dcb06619): coord req 20261016-1453 の前提の欠け 3 つを §9 に足した。
  - 人の承認で止まる演習の口(--human・--resume-session・--finish-session)
  - 工程の側の lock(PIPELINE_LOCKED・STEM_LOCKED・STALE_LOCK)
  - dec 欄は段で分ける(レコードの承認は thin-ledger も可・G6・G8・G11 は dec 番号だけ・工程は台帳を読まない)
  - 工数は 5.0〜8.75 人日。dkb の確認待ち(req 1452 の §2 追補)。coord への rep は確認の後。
- 改訂 2(08aa006a493efa8d・b81ae1e0・rep 20261016-1503):
  - dkb の確認(rep 1500)を反映した。条件 3 は (b) を採った = 適用の段で、当該 1 件の種別の辺に `source='retry'` を加法の SET で付ける。
  - 宣言の差は 2 件・4 本・`source` の欄だけ(前回の 1 件と今回の 1 件で向きが逆)。
  - 足したもの: 辺の単位の差分の道具・写しの数と sha の一覧・引数なしの回帰・演習の片付け(--list-sessions・7 日)。
  - 工数は 5.75〜10.0 人日。dkb に改訂 2 の再確認を依頼中。決263 で実装の承認を諮る予定。
- 改訂 3(b657aceac7ddd111・997da68a・rep 20261016-1508): dkb rep 1505 の是正を反映した。
  - 原因(確定): 信号の辺の evidence は、build_graph が value・apply_categories が span を入れる。
  - (b) は、apply_categories と同じ規則の値に DELETE なしの MERGE…SET で揃える形にした。
  - 期待する差は、種別の結果のファイルから規則で導く(欄を手で挙げない)。detail は差 0 を条件にした。
  - 前回の 1 件は直さない(決262 追補 7)。実装は決263 の承認の後。
- 改訂 4〜7(2026-10-16・現行 19bc2003a270fceb): rev TD262-1〜7 の是正(§10)と、他の所管の契約への揃え。
  - 承認の識別子: dec = thin-ledger:<seq>:<nonce 16 桁の乱数>(fed 442b2a1f)。書く順は flock → .pending → 台帳 → 付け替え。
  - 演習: 予約と profiles の正本は dkb 改訂 6(e13fa613)。識別子は秒の stamp + 乱数 8 桁・rehearse.id の札。
  - 再開: 待っている段の承認 1 本の追加だけを許す。REHEARSAL_CODE_MOVED(決262 追補 11)で、リンク先のコードの変化(未コミット・未登録を含む)を拒否する。
  - その他: --human-gates で停止点 1・2 を必ず止める。費用は実行前の固定と呼び出し前の予約(COST_PRICE_MISSING・COST_CAP・BaseException・AWS_MAX_ATTEMPTS=1)。最大は 12 試行。期待する差は実行前に固定し、多重集合で比べる。
  - 工数は 7.7〜13.1 人日。rev の再確認待ち → 決263。
- 改訂 8・9(2026-10-16・現行 f1110dd52cc29f0f): rev 再確認 R262-1〜3 の是正。
  - R262-1: 承認ファイルに共通 4 欄(run_id・gate・stem・target_sha = 見せた提案のファイルの sha)。fed へ契約の req(1632)。
  - R262-2: 予約を呼び出しの前に永続化し、確定 + 未精算 + 新規で判定する。結果不明は止める。入力の上界の根拠は利用者の判断待ち(COST_BOUND_UNCONFIRMED・決まるまで LLM を呼ばない)。
  - R262-3: dkb 改訂 8(d91816e0)の契約に揃えた。開始で未コミット・未追跡・無視の .py を拒み、再開で同じ検査をする。
  - 工数は 8.3〜14.1 人日。
- 改訂 10〜12(2026-10-16・現行 c255a1950e365f11):
  - 決262 追補 12: 入力の上界の仮定を承認した(呼び出しごとに実トークン数と照らす・COST_BOUND_VIOLATED)。Opus 4.7 の仮定の単価は c1dfd9bd(報告に「仮定の単価」)。
  - 改訂 11: profiles の写しは dkb(決263-B 追補 1)。bkb は再開の照合に含める側だけ。
  - 改訂 12: R262-1 の残件。停止点 3 では、見せた表示の sha(shown_sha256 → 束の view_sha)と承認の対象(target_sha = bundle_sha)を分けた。BUNDLE_VIEW_CHANGED を足した。
  - 工数は 8.15〜14.1 人日。fed の照合表がそろったら rev の再確認 → 決263-A。
  - **教訓**: 改訂 1 までの「既存の事故を同じ値で張り直しているだけ」は、正本の現状を数えずに書いた誤りだった。最後に入れた 1 件の辺には `source` が無く(1/685・1/669)、全件の経路は次の投入でそれを足していた。[[assumed-current-state-without-checking]]

**dkb の確認(rep 20261016-1500_dkb_to_bkb_rep_thin-plan-a-design-check.md・main af51914d)**: 条件 1・2・4 は満たす・§3-6 の推しに同意。条件 3 は方法に同意だが、そのままでは一致しない見込み。正本 B の種別の辺で source の無いものが 2 本(OF_RAIL 1/685・OF_SIGNAL 1/669・acc_id 最大の 1 件)。全件の経路は次の投入で source='retry' を付け、薄い経路は付けない。graph_digest は辺の性質を含む(export_graph_apoc.py 127〜128)。推し (b) = 適用の段で新しい 1 件の辺の性質を加法 MERGE…SET でそろえ、前回の 1 件は期待する差として宣言。ほか要対応 4(辺の単位の差分・写しの件数+sha・引数なしの回帰・残った演習の一覧と期限)・限定 1(保存の順)。coord は bkb の判断を決263 の前提に入れる(2026-10-16)。

**実装の状態(決263-A・2026-10-16 23:11 利用者承認)**:
- 実装は専用のブランチ `bkb-a263`(作業ツリー `/home/masam/jrtokai-phase12-bkb-a263`)で進める。dkb の受入の後まで main に入れない。bkb の作業ツリーは連絡文のためだけに使い、main へ早送りする。
- LOCKS は info 20261016-2312 で取った(期限 2026-10-22)。
- 段 2(finish の 4 本の絞り込み)は 22b9948a。試験 test_finish_scope は 8/8 で、変異で落ちることも確かめた。
- 設計 改訂 13(9a4a89c5): enrich_graph の末尾の cause_concepts は、絞った呼び方では呼ばない。dkb の確認待ち(req 2317)。
- 段 3 ingest_bundle 4c2fc4c2(28/28)・段 4 ingest_one 124b3a18(6/6)・段 5 ingest_pipeline 2d4892ed・段 6 llm_usage dda3e0d0(17/17・pipeline 18/18)。設計は改訂 15(f47905b9)。
- 段 7 rehearse_env a0f9812e(f79aec7351dfb1bc・dkb 17/17・human 13/13)。
- 段 8 同値性の演習 e9744a8c(equiv_thin_full.py): 種別の辺は期待どおり・**全件の経路が決95 の是正を 622 節点で巻き戻す(確定)**→ 改訂 15 の厳密な判定は不合格。設計 改訂 16 案(839574861a753838)で案 B(dkb が G16 を当て直して厳密に比べる)を推し、coord に要利用者承認の req 0031・dkb に req 0031。返事待ち。
- §4-3 の人の承認の演習は済み(2026-10-17 00:39・演習 20261017-003236-84a2e6e1・G0〜G10・info 0039)。dkb は rep 0033 で G16 の口(--reapply-unmask・dkb が回す)を可とした。
- 決263-A 追補 5(利用者 2026-10-17 01:07)= 案 B。--reapply-unmask 実装(988a963e・b4692fd03376816f・dkb が回す)・設計 改訂 16(9689f67bc3d482bc)・dkb へ受入の req 0109。
- **完了(2026-10-17 01:46)**: dkb 受入 合格(rep 0142・対象 31eba4a9・select_rewound を挟んだ版)→ 差分の要約 rep 0144 → main 3599e9e1 へ取り込み・LOCKS 解放・req 2311 閉じ。受入の限定 4 つ(dkb rep 0142 §3)。次は coord の rev 確認と決264(LLM を呼ぶ演習)の諮り待ち。
- (済)段 7 の rehearse_env(dkb の境界 req 2315・試験 test_rehearse_resume 17 本・予約の欄 id/mode/created_by/pid/started/containers・RehearsalStop.reason)→ 段 8 の同値性の道具(既存の事故の PDF を「追加」として偽の LLM で通す)。

**Why:** finish の途中の 4 本(apply_categories・log_dates・enrich_graph・build_fulltext)が全件を処理し直すので、絞らないと「承認済みの束だけを適用」が成り立たない。
**How to apply:** 承認の連絡が来たら、条件 1 の設計の確認から始める。実装の前に LOCKS を取って dkb へ info を出す。演習の環境の外では回さない。[[ingest-pipeline-enforcement]] [[v7-demo-ui-bypass-and-ner-raw]]

**受入 合格(2026-10-17 01:42・rep 20261017-0142)**: 対象 bkb-a263 31eba4a9。同値性は案 B(決263-A 追補 5: 全件の経路の DB に決95 の是正を当て直してから厳密比較)で合格・変異は不合格。**途中の発見**: 全件の再構築は data/ から作る欄だけを巻き戻す(Accident.summary などは是正の後のまま)ので、DB は「一部だけ巻き戻った」状態になり、表の全部を前提にする unmask_fields.plan(G16)は止まる。dkb が `select_rewound`(巻き戻った文書と欄だけに表を絞る・別の変化は止める)を足した(main 4c4539b4)。巻き戻りの機械の守り = `unmask_fields.py --check-rewind`・監査 X9b・基準は本体ツリー private の unmask_post_counts.json(欄 529・印 5,199)。独立の比較の台本は scratchpad の indep_diff.py(多重集合・値を出さない)。残り: main への取り込み(bkb)・LLM を呼ぶ演習は決264。
- rev 0309 = 要是正 R263A-1〜5(決263-A 追補 10)。bkb 分(R263A-1 上界違反の永続停止・R263A-4 工程の G2/G4 本文の契約)を bkb-r263a 4fce667e で是正・LOCKS 保持・dkb に確認の req 1040・fed に契約の req 1040・coord に rep 1040。→ dkb 確認 合格(rep 1044・別プロセスで送信 修正前 3→後 1)→ main 1855dc2a・LOCKS 解放・req 1037 閉じ(2026-10-17)。残りは fed の R263A-2〜5 と rev の再確認・決264 は coord。
- 決264 の材料 rep 1200(2026-10-17): 新しい PDF は無い(未登録の PDF 1 件は登録済みの写し)・候補 2 件(推し 方向回線 stem 71fe19d263cf6446・候補 1 = E 650b6b53855ae2ee)・名目 3 回で最悪 0.966 USD・stage1 のやり直しは COST_CAP・開始前に直す点 = rehearsals の置き場 0755 で names_proposal 0644(coord の可否待ち・dkb 所有)。

**2026-10-17 12:xx**: rev R263A-1〜5 是正済み(bkb 1855dc2a・fed e0d91076)・rev 再確認 1150 = 条件付き(残件 R263A-4a = 非空例の漏れ 0 の検査範囲・fed req 1155)。決264 の材料(bkb rep 1200): 新しい PDF は無く既存の写しを別名で・候補 2(方向回線・過去 NER 氏名 3)推し・費用 0.9663 USD(仮定)・rehearsals 置き場 0755 の是正が開始前。付議は 4a と是正の後。
- 決264 の開始前条件(rehearsals の写し先 0700/0600)を bkb-r264p 6c0a04b2 で是正 → dkb 確認 合格(rep 1206)→ main ebe5a62a・LOCKS 解放・coord に rep 1207(2026-10-17)。既存の記録 55 は dkb が絞った。bkb の手番なし。決264 は fed の R263A-4a が閉じてから coord が諮る(候補 2 件・推し 方向回線)。

**決264(2026-10-17 12:3x 承認)**: LLM を呼ぶ隔離演習を開始・候補 2(方向回線・eb746c0de18c1615)・本人 1 名が全段・版 main 6e1adf82・上限 1 USD(仮定単価・上界 0.965485・再試行は通り得る)・手順書を bkb(工程)/fed(窓口)に req → coord が 1 本に → 利用者が手元で回す。
- 決264 承認(dec 20261017-1356・候補 2・利用者本人が全段・版 main 6e1adf82)。工程側の手順書 docs/手順_LLM隔離演習_工程側_bkb_20261017.md(0b0df43ad0e4f099・16b21a43・rep 1402)。本体ツリーで回す(rehearse_guard が固定ツリー不可)→ 演習の間はコードの取り込み停止を開始前条件に。対象の写し = private/dec264/dec264_hoko_c2.pdf。coord が fed の窓口側と 1 本に揃えて利用者へ。
