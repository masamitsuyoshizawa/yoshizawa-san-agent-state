---
name: facets-instance-entry-issue
description: facets の設備クラス軸に個体 entry(転てつ器164号 など)が候補として混入し 0 件の組を作る問題。2026-09-28 に整理 md を作成・実装は後日(利用者指示)
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-28T13:39:26.021Z
---

利用者の質問(2026-09-28)で判明。「四日市駅の164号ポイントの転換不能」→ equipment_class の候補に `dict:eq:ee328e92 転てつ器164号`(別名「164号」・proposed・parent eq:分岐器)と `eq:分岐器`。個体の組は 0 件・クラスの組は 8 件。
- 原因: candidate-rule-v1 が entry の粒度を見ない(`_candidates_eq` は当たった entry を全部)+ 語彙 DB v3.0 の設備 entry 3,554 にクラスと個体が混在(正準名に番号 212・親が設備 entry 112・親なし 100・別名が N号 だけ 16)。
- 整理 md: `docs/検討_facets_個体entryの候補混入_20260928.md`(案 A 軸ごとの振り分け・親へ畳む / B 辞書に粒度の印 / C 番号だけの別名を外す / D 現状+ガイド。coord の見立ては A を主に B で裏づけ)。
- **利用者: 「修正したい。まず整理の md。実装は後日」**。計画はまだ起こしていない。担当の見立て: fed(facets)・dkb(辞書)・bkb(受入)・coord(共通表 §1-15 改訂)。
- 特異点(dkg_backend._singularities)は組をまたいで事故の異なりを数えるので二重計上は無いが、組の上限 90 の圧迫は共有。

**Why:** 切り口の名前(クラス)と中身(個体)がずれると、エージェントが 0 件の組を「同種事象なし」と誤読する。
**How to apply:** 実装の指示が来たら、この md §6 の決めること(個体の判定基準・親への畳み方・same_equipment での個体語の要否・契約改訂・受入)から計画を起こす。関連 [[agent-docs-bkb-dkb-20260928]] [[facets-input-regex-pitfalls]]

**改訂 1(2026-09-28 15:1x・main 4caec23d・sha16 bdd37ff193cc5b5d)**: bkb の受入 7 点(独立の期待集合・基準の誤りを層化標本で・失った事故を id で・辞書の並び不変・受け取る側ごと・candidate_rule_version 上げで vocab_ref 呼び手が 422・同じ環境と束)と合成の質問の型 6 つ、fed の 2 点(増える組 = (3 × 現象候補数 + 1)× grouping・G7 は無関係)を §6-5/§3/§5 に取り込み。fed/bkb とも原因の記述は現物と一致。教訓: 早送りが失敗したまま SendMessage で「済み」と言った(bkb 指摘)→ ff=0 を見てから言う。

**dkb の事実(15:07・main 66a7e985・sha16 1b90deb90273b262)**: 設備 entry の level(system 9/device 1,100/unit 588/part 1,739/model 118)は部分と全体の段で粒度の印ではない(転てつ器164号 = unit・分岐器 = device)。status は confirmed 0/proposed 2,409/欄なし 1,145 で使えない。案 B は level と別の欄が要る。

**決134(2026-09-28 21:38・main badf3392)**: 利用者「検討書に従って修正」。coord の前提 = 案 A 主(equipment_class/location_class で個体 entry を外し親へ畳む・same_equipment* は従来)+ 案 B 裏づけ(辞書に granularity 欄・level と別)。判定基準は dkb の層化標本(212/100/16/無作為 100・札 クラス/個体/規定/他)の数を見てから利用者に諮る。req 2138 ×3: fed = candidate-rule-v2 計画・dkb = 札 + 辞書欄の設計・bkb = 受入設計。揃ったら rev → 利用者承認 → 実装 → 受入 → EC2(別承認)。触らないもの: DSL・正本・語彙 DB・共通表本文・EC2。

**決134(2026-09-28 21:38)で修正に着手・dkb 1b を rep 2149 で返した(main 7d712dce)**: 計画書 docs/計画_設備辞書の粒度の印_dkb_20260928.md(改訂 0・1270afaeef8bdfd6)・道具 granularity_sample.py(sample/count)・CSV は本体ツリー knowledge_kb_v8/data/eval/granularity/(5a0d27b12d7e1132)。層 S1 112/S2 100/S3 60(正準に番号なし・別名に番号)全数 + S4 3,282 から 100(種 20260928)。**札は dkb(LLM)の判定案 = 人の確定が要承認**。判定案: (i) 単独不可(偽 約 1,400)・(ii) 適合 177/212・見逃し 約 230(再現率 約 43%)。案 B = Equipment.granularity(class/instance/regulation/other/unreviewed)・v4.0・読み手(fed)の切替は投入と同時。**neo4j-admin dump/load は保存の順も写す**(並び完全一致を実測)→ 順の試験 d2 には使えない。次: 人の札の確定 → count --col human_label で数え直し。

**手順 1 の成果(2026-09-28 21:4x〜21:5x)**: fed 計画 candidate-rule-v2 85b1a07bd0eed269(クラスの軸 = 個体でない候補 ∪ 畳み先・same_equipment* は v1・親なし個体は外す・組は増えない/strong 不変)・dkb 計画 1270afaeef8bdfd6(granularity 5 値を Equipment の欄・公開版 v4.0・vocab_db_reader の切替が要る・(i) 単独不可・(ii) 適合 177/212 見逃し約 230)・bkb 受入 改訂 1 25ac6705d0a5631a(受K0〜K9・|L|=0・Q-loo 647 全問・Q-t0 19・digest 固定)。**決135**: 札は rev の独立判定案と dkb 案の二重で絞り、不一致 + 低確度 29 だけ利用者確定・unreviewed はクラス扱い(D0)。rev へ req 2154(3 計画の確認 + 盲検の札付け)。load-rehearse は保存の順も写す → 受K4 d2 は逆順書き道具(dkb 実装段)。検討書 改訂 2(34号→転てつ 訂正)。

**rev の静的確認 rep 2215(2026-09-28 22:15・代行コミット 2e271475)= 要是正 R1〜R8**: R1 粒度 5 値と D0 の契約(F は 4 値で拒否・保存値 5 値を残し unreviewed の候補規則だけ class 相当が推奨)・R2 受K1 を再帰的 fold `(C1\I) ∪ fold(C1∩I)` に(10 辺の境界・system/循環の判定順)・R3 組数非増加は導ける(旧 g(3sp+a)・新 g((2t+s)p+a))が strong 不変は無条件でない(too_many_groups→strong)・R4 受K3 の grouping 別集合式・未取得を 0 にしない門・647/646・R5 null 候補と facet_keys・R6 語彙 v4 は版対応読み手 (a) 先行・422 理由は vocab_digest/candidate_rule_version・R7 番号だけの別名は 35 行/28 entry(F の 35 は単位混同)・R8 並び変異 M-first/M-seq に順序差が届く条件。4 質問集合の digest 一致。**coord は req 2233 ×3 で振り分け(fed R1/2/3/5/6/7/8 → 改訂 1・dkb R1/6/7 → 改訂 1・bkb R2〜R8 + 既知 3 点 → 改訂 2・対応表 2 枚は fed 起こし)・main dadd859d**。**(B) 盲検 CSV は rev が AGENTS §A-2 で本体ツリーを読まない(info 2208)→ coord が全値を追跡中の dict_equipment.json(94885af182efdebd)と照合(371 完全一致・1 は改行→空白)して `docs/review_rev/inputs/granularity_sample_blind_20260928.csv`(同 sha16 2a20a8364bd033f3)に git 追跡で配布(info 2238)**。次: 3 者の改訂 → rev 再確認 → §12 の 5 点と札の確定を利用者へ。

**fed 改訂 1(2026-09-28 22:45・main f90c4147・sha16 4f2dd0e83bdf4b9c)**: rev rep 2215 の R1〜R3・R5〜R8 を全部採った。粒度 = 保存値 5 値 + 欠欄 null を応答にそのまま・unreviewed/欠欄は挙動だけ class(決135)・T = NUM_CANON 212 ∪ NUM_ONLY 別名の持ち主 28 = 216 id の中の unreviewed/欠欄を未確定として数える。fold の順 = 親なし→辞書に無い→invalid→循環→11 辺目→system→個体なら上へ→regulation/other 停止→class/unreviewed/欠欄を採る(10 辺先は採る)・fold_stop 欄。strong 不変は条件つき(100→64 組の反例)・T7 は 7a 不変/7b also_in は再計算。付録 A/B(読み手は全ノード=履歴も形検査するので v4 公開取り下げでは戻せない→dump 復元・読み手を戻すのは DB の後)。dkb/bkb へ照合の req 2245。次は rev 再確認 → §12 の 6 点を利用者へ。**教訓(R7)**: 別名の「行」を entry と書いた(35 行 = 28 entry・クラス側 11 行 = 4 entry)。数える単位(行/id の異なり/表層の異なり)を表の列に書く [[report-counts-with-scope]]。

**3 者の改訂 出揃い(coord・2026-09-28 22:42〜45・main 30fe7adf)**: fed 改訂 1 4f2dd0e83bdf4b9c・dkb 改訂 1 a5cb4c7d3fc08e5d(granularity_source 新設案 double_draft/user/pending/none・対象内の門・write_phase2 は同じノードを SET で書き換える=既存ノードに欄が載る・JSON と DB の層 372 一致)・bkb 改訂 2 841d0ed76d9e8614(fold_B・5×5 遷移・未取得の門・647 問+exclude-ids-digest・受K9 3→8)。**coord が見つけた食い違い 2 点(info 2247)**: granularity_source は dkb 新設/fed 削除(DB の欄なら読み手の宣言が要る・B1x)、fed 付録 B-4 の 1 は dkb rep 2242 で回答済み → B-2 の 3(granularity_before_v4)は成立しない。次: dkb・bkb が付録 A/B を照合(fed req 2245)→ fed 改訂 2 → coord が §12 の 6 点 + dkb スキーマ(granularity_source・門)+ bkb の記号(FOLD_TARGET・EDGE_AT_MAX)を 1 通の dec 案に束ねて利用者へ → rev 再確認 → 実装承認。**この記憶ファイルは fed・dkb と共有されており同時書きで段落が消えうる(dkb の 22:4x の段落が消えた)。追記は末尾 append で。** 教訓: 受領注記の時刻を手で 22:48 と打ち先行させた(訂正済み)。

**fed 改訂 2・3(2026-09-28 22:50〜22:52・main a8d3a130・改訂 3 sha16 e0e77498ac0dc221)**: granularity_source は語彙 DB/JSON に置かず dkb の台帳だけ(案 c・DB は unknown_property・JSON は I0 で明示して止める。いまの JSON 読み手は未知の欄を黙って通す=実測)。v4.0 は既存ノードに欄を SET で足す(dkb 確定)→ 取り下げでは戻せない・戻しは dump だけ。読み手は VocabRelease.major で判定し主番号 3 で欄が在れば止める(禁じた道の検出)・旧版の digest 読み直しは道具が主番号で投影。加法の欄は v2 のときだけ(v1 の B1〜B4 は形不変・T0b の除く欄は版 5 つ)。門は T 216。§12 は 7 点。照合の食い違いは残っていない → coord が利用者へ諮る。

**決136(coord・2026-09-28 22:58・main fb01948e)= 設計 4 点承認(全部推奨どおり)**: (1) 印なし parent を上へたどる畳みを展開と別の新規則として認める。(2) 細目は fed 改訂 3 e0e77498ac0dc221 の案(畳み先 class/unreviewed/欠欄・親なし個体は外す・停止順 9 段・10 辺先を採る・欄名 granularity/folded_to/folded_from/fold_stop/reason_code 新値)= bkb FOLD_TARGET={class,unreviewed,null}・EDGE_AT_MAX=10。(3) スキーマ: granularity 欄 v4.0・版対応読み手先行(戻しは投入前 dump のみ)・granularity_source は語彙 DB/JSON に置かず dkb 台帳だけ(c)・別名の札は別件。(4) 受入の門 T=216 の未確定 0。未決 = 判定基準(dkb §6-2・札確定後)。fed 改訂 3 の実測: JSON の読み手は未知の欄を黙って通す(I0 で明示して止める)。bkb 改訂 4 5d7834c21adce022(rep 2254)。次: dkb 改訂 2・bkb 改訂 5(要れば)・fed 追補 → rev 再確認 → 実装承認(別 dec)→ 受入 → EC2(別承認)。

**決136(22:58)設計 4 点承認 → dkb 改訂 2(104274e4・rep 2301・main 04f4bdfa)**: granularity を Equipment に新設して v4.0 にする。granularity_source は語彙 DB と JSON に置かず、台帳 knowledge_kb_v8/config/equipment_granularity_ledger.csv にだけ置く(JSON と突き合わせて止める)。門は T = 216(NUM_CANON 212 ∪ NUM_ONLY 28)の未確定 0。戻しは投入前の dump だけ。読み手は R だけを読み、主番号(VocabRelease.major)の投影は digest の道具に入れる。v4.0 では既存ノードに欄が載る(その場の書き換え)。未決 = 判定基準のみ。実装・演習・投入は別承認。次 = rev の札 → 段取り 3。

**決136 反映の 3 本が揃い rev へ再確認(coord・2026-09-28 23:03・req 2303・main b105f1d6)**: fed 改訂 3 追補 1 344e77e940c5dfb3(設計の変更なし)・dkb 改訂 2 104274e43800eb6a((c) 台帳 knowledge_kb_v8/config/equipment_granularity_ledger.csv git 追跡・門 T=216・S3 は T 内 4/外 56・§6 は判定基準だけ未決)・bkb 改訂 5 99c828299aad7657(式と値は改訂 4 のまま・文の直し)。未決 3 = 判定基準・束 digest の算法の名(dkb)・受K3 の合否の案。次: rev の rep(合格/条件付き/要是正)→ 実装の承認を別 dec → 実装 → 受入 → EC2(別承認)。教訓: req 2303 の re: を fed rep 2259 の想像した名で書き、ls で照合して訂正した(綴り誤りの再発)。

**束 digest の算法の名は据え置き(dkb 決定・2026-09-28 23:05・rep 2305・計画 改訂 3 9a0c566406154df6・main ed0d7bdd)**: 主番号 4 の束も vocab-bundle-digest-v1(算法は符号化・整列・連結・sha256 だけ・属性一覧はスキーマ側)。条件 = 3 実装とも投影を読む版の主番号で決める。検出 = dkb/bkb 突き合わせ + v3.0 読み直しで 2e763fba 再現。rev の req 2303 に追補 1(対象を改訂 3 に・十分性も見る)。未決は判定基準・受K3 の合否の案の 2 点。

**段取り 3 の集計(23:26・rep 2326・main d82101ad)**: rev 案は docs/review_rev/inputs/granularity_sample_rev_20260928.csv(96b49659)。一致 358/372(κ 0.932)・不一致 14(I↔O 7・C↔O 5・C↔I 2)で、両者高確度の不一致は 0。確定対象 = 不一致 ∪ dkb 低確度 = 37(T 内 23)・rev 低確度の追加 43(T 内 31)・一致かつ両高 292。一覧は git 外 granularity_double_check_20260928.csv(e4a1fa87)。次: 利用者の確定 → count --col human_label → 判定基準の案。算法の名は据え置き(改訂 3・9a0c5664)。

**rev の (B) 札付け 完了(2026-09-28 22:52・rep 2252・代行コミット 9a512350・main fd468174)**: docs/review_rev/inputs/granularity_sample_rev_20260928.csv(git 追跡・96b49659412b50d9・372 行・class165/instance179/regulation2/other26・低確度 67 行=rev_basis 先頭「低確度：」)。coord が構造(8 列不変・4 値・根拠非空・層別)を独立再計算して一致。rev の提案(要承認): rev 低確度 67 も確認対象の和集合へ(二重一致でも同方向の推測がありうる)。→ dkb へ段取り 3 の req 2325(一致/不一致・不一致∪dkb 低確度 29・rev 67 の重なり・一致かつ両者高確度の数)。数が出たら利用者へ諮る(確定対象に 67 を足すか・一致行を暫定確定とするか)。rev の (A) 再確認 req 2303 は未回答(rep 2252 は 22:52 で req 2303 の前)。

**dkb 段取り 3 と決137(2026-09-28 23:26〜29・main b7d7956d)**: 二重判定案 一致 358/不一致 14(κ 0.932・不一致は全部片方が低確度・I↔O 7/C↔O 5/C↔I 2)。分割 37(不一致∪dkb 低確度)+43(rev 低確度で外)+292(一致高確度)=372・T 内 23+31+162=216。一覧は git 外 granularity_double_check_20260928.csv(e4a1fa8789a92ba6)。**決137(全部推奨)**: 利用者確定 80 行(rev 提案採用・T 内 54)・292 行は double_draft で暫定確定・CSV 切り出し方式(dkb が 80 行の確認用 CSV を git 外に作り利用者が human_label を埋めて返す・req 2329)。次: 利用者が CSV を埋める → dkb が count --col human_label → 判定基準(dkb §6-2)を諮る。rev の再確認 req 2303 は並行・未回答。

**rev 再確認 rep 2337(2026-09-28 23:37・代行コミット・main 45e1b831)= 要是正 N1〜N5**(R1〜R8 の中核は解消): N1 台帳専用欄 granularity_source を D:210/B:298 が DB/JSON へ要求する旧文(dkb/bkb)・N2 台帳と JSON の granularity 同一性のモデル検査不足(dkb・反例 = 台帳 class/JSON instance で門を通る)・N3 fold-only 親選択時の null ANY は not_selected でなく skipped/missing_elements(fed)・N4 特異点遷移表に not_checked が無い → 6×6(bkb)・N5 fold_B の戻り対と != None の不整合・停止理由 8→7 種(bkb)。§5 後着決定の整合化(D:250/267 29→80 行・D:309/315 & B:130/134 の 372 件 → 80 + 292 double_draft・T 内 54 行は返却前に未確定 0 にしない)。rev の見立て: 判定基準は札生成の実装前に確定・受K3 の合否案は判定器実装前(遅くとも本測定前)に承認・v3 再現だけでは v4 投影の証明にならず D:208 併用。coord は req 2348 ×3 で振り分け(fed N3+注記・dkb N1/N2+後着・bkb N1/N4/N5+後着+注記)。次: 3 者の改訂 → rev 3 回目 → 実装承認(別 dec)。

**N1〜N5 反映(2026-09-28 23:50・main 7c74583c)**: fed 改訂 4 bd08c4e7dbb02615(N3 の期待分け・T11 4 通り・strong 条件・T16 検出十分性 3 点)・dkb 改訂 4 cbe88b3dfce396e5(N1 両経路 granularity だけ・N2 台帳突き合わせ L1〜L5 + 否定例 3・決137 集計 §9-3 採用札の表 372 行 adopted_label/adopted_source・T 内 54 は pending で門が v4.0 を作らせない)。bkb 改訂 6 待ち → rev 3 回目 → 実装承認 dec(rev の期限 2 点も一緒に諮る)。

**3 本揃い・rev 3 回目(2026-09-28 23:54・req 2354・main 6219b2c8)**: fed 改訂 4 bd08c4e7dbb02615・dkb 改訂 4 cbe88b3dfce396e5・bkb 改訂 6 2a06e5dea3b33fbf(N1 単独投影・N4 6×6・N5 target_B/stop_B 7 種・受K2 は dkb §9-3 に揃え・受K0 (a)〜(c)・§7 受K3 承認は判定器実装前)。rev 合格/条件付き → 実装承認 dec(rev §5 期限 2 点込み)。利用者 CSV 80 行は未返却。

**rev 3 回目 rep 0000(2026-09-29 00:00)= 条件付き・決138(00:09・main 7e8df032)= 実装承認(範囲限定)**: N1〜N5 は計画上解消。残る C1(受K2b の照合先を human_label → adopted_label・T 内 162 は double_draft で 80 行では照らせない)・C2(vocabulary_unavailable を仕様どおり skipped に入れない・service_state != ready が 1 件でも全体保留)は bkb 判定器実装前・coord の現物確認で閉じる(req 0008 → 改訂 7)。**決138**: 実装範囲 = fed I0 + v2(既定 v1・ローカル)+ T0〜T16 / dkb 台帳・L1〜L5・T の門・v4.0 道具・投影(投入・公開・札生成は別承認)/ bkb 判定器・独立投影(改訂 7 後)。受K3 合否案 承認(|L|=0 合格・L≠∅ 保留で利用者へ・説明不能な利得も保留・未取得 1 件で全体保留)。期限 2 点固定(判定基準は札生成の実装前・受K3 は判定器前で替えない)。順: bkb 改訂 7 → 3 者実装 → 利用者 CSV 返却 → 判定基準 dec → 札生成・v4.0 演習/投入 dec → 受入測定 dec → 既定 v2・EC2 dec。共通表 §1-15 改訂案は coord(受入前)。

**決138(2026-09-29 00:09)の実装承認 → dkb 分を実装(38608eb4・rep 0024・main ef32262b・ローカルのみ)**:
- 設計書 追補版 13(592b4118)を入れた: 内1 に granularity・札 20・VocabRelease.major・算法の名は v1 のまま。
- vocab_model: 主番号は JSON から推定する(全部 4 / 0 なら 3 / 一部なら止める)。L1〜L5 と T の門。台帳 LEDGER_PATH はまだ作っていない。
- vocab_db: read_bundle(at=) で旧版を投影して読む。
- vocab_gate: build_model で SCHEMA_MAJOR を照合する。
- 試験 test_vocab_granularity 28 件・変異 5 つとも検出。一時 DB の v3.0 は digest 2e763fba を再現し、v4 の後の読み直しでも戻った。
- 教訓: 札の名の正規表現 [A-Za-z_]+ は granularity_before_v4 を黙って外す(fed の D5 と dkb の codec の両方)。変異の写しは REPO が解決できず別の理由で落ちたので、理由の行まで見る。
- 次: 利用者の CSV 80 行の返却 → adopt と数え直し → 判定基準 → 札の生成と v4.0 の演習・投入(別 dec)。

**C1/C2 閉鎖・dkb 実装完了(2026-09-29 00:11〜00:24)**: bkb 改訂 7 09f65f50bcc0dbc7(受K2b = 束・台帳・採用札の表の 3 つの一致・human_label と照らさない・受K3 に語彙が使えない応答の門)を coord が現物確認して C1/C2 閉鎖(rev 再審査なし)。dkb 実装 38608eb4(granularity 受付・主番号推定・L1〜L5・T の門・札 20・主番号別投影・試験 28 + 既存 4 本・変異 5・一時 DB で v3.0 2e763fba 再現・codec 試験の札名 regex [A-Za-z_]+ を是正)。fed は実装着手(LOCKS: facet_backend.py・vocab_db_reader.py・順 2 の前提 = dkb の語彙 DB 設計書 追補 → dkb rep 0015 で回答)。bkb は判定器・独立投影の実装へ。残り: 利用者 CSV 80 行 → 判定基準 dec → 札生成・投入 dec → 受入測定 dec → 既定 v2・EC2 dec。共通表 §1-15 改訂案は coord。

**bkb 実装完了(2026-09-29 00:27〜31)**: 主番号別投影 run_v3_bundle_digest.py ce980b314625b60d / v3_vocab_projection.py 5f5d1adc91bf1108(コミット 208e6366・主番号 3 の出力はバイト同一・dkb 合成写し A/B/C と一致 rep 0031)・受入判定器 knowledge_kb_v8/scripts/bkb/facets_v2_acceptance.py 4d03f1219e73fe40(コミット 0d920128・畳み 9 段停止 7 種・K1/K2/K2b/K3/K5/K7・否定例・自己試験 49/49)。応答を集める台は fed の v2 後。残り = fed 実装・利用者 CSV・判定基準 dec。

**決138 実装完了(2026-09-29 00:37・main 832bfb65・rep 0037)**: facet_backend 3ae9d755e2a0373b・vocab_db_reader 8cb6f159bd3e6b22(追補版 13 592b41183e2d7ca9)・旗 KG_FACETS_CANDIDATE_RULE(既定 v1・起動時に読む)。試験 knowledge_kb_v8/scripts/fed/test_candidate_rule_v2.py 37/37・否定例 16・v1 は変更前と byte 同一 641 問×4 モード。実 DB 記録 r2 8d12f78e1150a2f1(読み手の照会に major を足すと凍結試験の再生が照会文で引くので取り直しが要った)。未決: 表示側 SG_REASON_LABEL に instance_only の文言(要承認・v2 既定化の前)・T16 の 3 者は v4.0 の後・既定 v2 化/EC2 は別 dec。教訓: 読み手の定数は dkb の設計書と試験で両向き照合されるので、欄や札を足す前に設計書の追補を頼む。

**受K7 の vocab_snapshot_id(2026-09-29 00:40〜)**: 別プロセス比較で必ず出る差 → fed 改訂 4 追補 1 d73681a092694042 で除く欄 10 に(プロセス固有の識別子・vocab_digest は除かない)・bkb 改訂 8 db62f1f68ccacbcc・判定器 bf7a153fc6f9043c(51/51)・空回し不一致 0。決138 の範囲は全部完了。次の承認: 判定基準(利用者 CSV 80 行の返却後)→ 札生成・v4.0 演習/投入 → 受入測定 → 既定 v2・表示文言 0.3.10・EC2。

**fed 実装完了・決139(2026-09-29 00:37〜42・main 9f40cfbf)**: facet_backend.py 3ae9d755e2a0373b・vocab_db_reader.py 8cb6f159bd3e6b22(dkb 設計書 追補版 13 592b41183e2d7ca9)・試験 37/37・否定例 16・既定 v1 は変更前 aa21932a と byte 同一(641 問×4)・v3.0 digest 不変・旗 KG_FACETS_CANDIDATE_RULE。**決139**: instance_only の表示文言「候補がどれも個体の設備で、同種の設備の切り口に使えない」を承認(表示側 3 つ = dkb 所有・プラグイン 0.3.9→0.3.10・U2-v2 は fed・EC2 は v2 既定化と一緒に別承認)。bkb 収集の台 run_facets_v2_collect.py 97db0a05(空回し・受K7 で vocab_snapshot_id の差 7 問 → fed req 0040・(a) 除く欄に足す推し・プロセス固有 id と明記)。決138 の範囲は 3 者とも完了。残り: 利用者 CSV 80 行 → 判定基準 dec → 札生成・v4.0 演習/投入 dec → 受入測定 dec → 既定 v2・表示文言・EC2 dec。共通表 §1-15 改訂案は coord。

**T16 と決139(2026-09-29 00:43〜00:45)**: 合成の札の一時 DB で T16 の 3 者の一致 = A 6320f015・B 5993d238 とも一致(rep 0043)。B の取り下げは v4.0 に戻っても中身は v4.1 の書換えのまま → 台帳照合で registry_mismatch(戻しは dump だけの実例)。決139: 表示側 3 つ(fed 所有・OWNERS 40〜42 行)に instance_only の文言・プラグイン 0.3.10(DSL は 0.3.9 のまま・EC2 と一緒に)。受K7 の vocab_snapshot_id は除く欄へ(改訂 4 追補 1)。

**決139 追補 1(2026-09-29・main 8313fdcf)**: 表示側 3 つの所有は fed(OWNERS 40〜42 行)。fed が文言追加・0.3.10・U2-v2。bkb 改訂 9 0adcadaed1d7f78e(受K0 に vocab_snapshot_id は束の同一判定に使わないと明記)。

**決139 実装済み(fed・2026-09-29 00:45・main 709e8c17)**: instance_only 文言を 3 か所に(dkg_diagnose 22435b72515d10bf・manifest 0.3.10 dfa24f23ebca5775・demo_shirei 76e31d519fb193b6・demo_v10 1bfd1fad2ecde34f)・U2 v1(7 札)/v2(8 札)PASS・DSL の依存識別子は 0.3.9@29ec8e22 のまま(0.3.10 の識別子は EC2 で包みを作ったとき)。T16 の 3 者一致は合成札で済(A 6320f015・B 5993d238)。fed は LOCKS を事後記録(自己申告)。**ローカルの実装は全部完了。残りは利用者の CSV 80 行**。

**利用者 CSV 返却(2026-09-30 00:54)**: granularity_confirm_20260928_excel.csv dd713eacb54d2e40(utf-8-sig・CRLF・80 行・14 列不変・human_label 全記入 I40/C27/O12/R1・T 内 54 全記入 I36/C12/O6・note 0・dkb 案一致 72・rev 案一致 74・両案と違う 0)。coord → dkb req(採用札の表 372・count 数え直し・判定基準 §6-2 の案)→ 利用者へ判定基準 dec → 札生成・v4.0 演習/投入 dec → 受入測定 dec → 既定 v2・EC2 dec。

**返却の取り込み(2026-09-30 01:20・rep 0120・main 405a52cd・needs_user_approval: yes)**:
- 返却版 dd713eac は utf-8-sig・CRLF で、Excel が引用符を外していた。
- 採用札の表 granularity_adopted_20260930.csv(2a7fb036・372 行・C164/I184/O22/R2)を adopt(granularity_sample.py f5cb7f6d)で作った。
- (ii) の適合は 173/212=81.6%。T の 216 は全部に札が付いた。
- 推し = (iii) 採用札。T の外に約 223 件(109〜436)の個体が残る。追加の選択肢 = 型(…踏切/TR-数字/漢数字の第)で 114 件を拾い人が確定。
- 教訓: cp932 の往復で「〜」→「～」に変わる(表せない文字 0 でも同一には戻らない)。否定例の写しは文字列置換でなく CSV として壊す(引用符の有無で置換が空振りし「通った」と誤読しかけた)。スクリプトの置換が assert で止まった後に古い版を実行してしまった → 置換が通ったのを見てから実行する。
- 次: 判定基準の dec → 札の生成・台帳・v4.0 の演習/投入(別承認)。

**決140・決141(2026-09-30 01:2x)**: dkb rep 0120 = 採用札の表 372(2a7fb036b117bb76・user 80/double_draft 292・C164/I184/O22/R2)・(ii) 173/212=81.6%・(ii)+除外 91.0%・T 216 全部に札・標本外の個体 約 223(109〜436)。**決140**: 判定基準 (iii) 採用札を印に・(ii) は T の定義として残す・T 外 156 件も入れる・追加札付け 114 件は今回しない(次の巡・顧客確認シート粒度列と)。**決141**: (a) 札生成(dict_equipment.json granularity)・(b) 台帳 equipment_granularity_ledger.csv・(c) L1〜L5 と門の実走・(d) v4.0 演習(一時 DB・3 者 digest・v3.0 再現)を承認。投入・公開・registry は演習結果を見て別 dec。cp932 往復は「〜」→「～」で止まる(dkb 実測)。

**受K2 完了(bkb・2026-09-30 01:23・rep 0123 to dkb)**: 3 入力から独立に作った採用札の表が dkb の 2a7fb036 と 372 行全一致・取り違え 36 升も一致。受K2b・T16 実物は dkb の演習(決141 (d))と台帳の後。

**決140/141(2026-09-30 01:26/01:27)→ dkb の (a)〜(d) 完了(rep 0140・main 68bb1639)**:
- 判定基準は (iii) 採用札。追加の 114 件は次の巡。
- (a) 辞書は 338ea0b1。札を外すと 94885af1 に戻る。統制の台帳を dec 20260930-0127 で再登録した。JSON 経路の vocab_digest は 8bc9a651 になった。
- (b) 台帳 80d9c595(372 行)。
- (c) L1〜L5 と門が本物で通り、否定例 8 通りも期待どおり止まった。
- (d) rehearse_env に --vocab-base-dump(v3.0 の dump を土台)を足した。passed=True。v4.0 の束 digest は ec5a3ade534840ee、v3.0 の読み直しは 2e763fba。
- D-KB の LOO 646 は札つきの辞書でも不変。
- **教訓: X13 の未許容(audit_dkb.py の一時 DB の URI)が 09-27 から main に残り、G0 を割っていた**。URI は rehearse_env.local_bolt で作る。道具を足したら X13 を回す。
- 一時 DB neo4j-rehearse-vocab(18690・ボリューム jrtokai_neo4j_rehearse_vocab_base)と接続先 private/rehearse_conn_vocab_20260930-0138.json は、bkb/fed の照合の後に消す(LOCKS 保持中)。

**決141 (a)〜(d) 完了(dkb・2026-09-30 01:40・rep 0140・main 68bb1639)**: 辞書 dict_equipment.json 338ea0b1347af6ef(コミット 296549ce・札 class164/instance184/regulation2/other22・unreviewed 3,182・札を外すと 94885af1 に戻る・vocab_digest JSON 経路 8bc9a6514042a91a)・台帳 knowledge_kb_v8/config/equipment_granularity_ledger.csv 80d9c595726ff341(372 行・id/granularity/granularity_source/decided_on/sample_row/note)・manifest 再登録(dec 20260930-0127)・L1〜L5 と門 通過(T 216 未確定 0・否定例 8 通り)・演習 passed(rehearse_result a8d8291902b070b9・G14 update_nodes 3,554 のみ)・v4.0 束 digest ec5a3ade534840ee・v3.0 再読 2e763fba・正本不変(6de974dc)・LOO 646/646 不変・X13 未許容(audit_dkb.py の一時 DB URI・09-27 から)是正。一時 DB neo4j-rehearse-vocab(18690)を bkb/fed の照合まで残す。次: 照合 → 投入 dec(dump → commit → 公開 → registry)。

**bkb 照合 合格(2026-09-30 01:42・rep 0142)**: v4.0 束 ec5a3ade・v3.0 再読 2e763fba が dkb と一致・一時 DB 受1〜受8 一致・受K2b 372 行不一致 0・T 未確定 0。fed(3 者目)待ち → dkb が締め → 投入 dec。

**決142(2026-09-30 01:52)でローカル正本に v4.0 を投入(rep 0155・main dd4709e5)**:
- 投入前の dump: pre_v40_dump_20260930(86489139・戻しの土台)。
- commit の到達は 4 つとも成功。台帳 vocab は v4.0(ord 2・ec5a3ade)。
- 正本の束は ec5a3ade、v3.0 の読み直しは 2e763fba、全件 hash は 9e36fa44。
- 書き出し: bundle_v4.0_r2.json(7e8beded)。
- 残り: bkb・fed の正本の照合 → LOCKS 解放。EC2 と既定 v2 は別 dec。

**3 実装一致・決142(2026-09-30 01:48〜52・main 07885cb2)**: fed T16 実物 ec5a3ade 一致・札 3,554 一致・field_missing 拒否(rep 0148)・dkb 締め(rep 0149・一時 DB 片付け・LOCKS 0)。**決142**: ローカル正本 10190 へ v4.0 投入を承認(dump → ingest_pipeline --mode commit --payload vocab --vocab-release v4.0 → registry v4.0 → 書き出し ec5a3ade/2e763fba → bkb/fed 照合 → LOCKS 解放)。EC2・既定 v1・Dify 不変。次: 投入完了 rep → 受入測定 dec(bkb)→ 既定 v2・0.3.10・EC2 dec(fed→coord)→ 共通表 §1-15 改訂案(coord)。fed の凍結試験 DB 経路は投入後に正本へ切替。

**v4.0 投入完了(dkb・2026-09-30 01:55・rep 0155)**: 正本 10190 に v4.0 投入。dump 86489139(v3.0 2e763fba・停止 15.5 秒)・commit rc 0(状態 7e2f4831・G14 update 3,554)・台帳 vocab v4.0(release_ord 2・ec5a3ade・change_count 3,554)・正本 束 ec5a3ade534840ee・v3.0 再読 2e763fba・全件 hash 9e36fa44。bkb/fed 照合(手順 5)→ LOCKS 解放 → 締め rep。EC2・既定 v1・Dify 不変。

**bkb 正本照合 合格(2026-09-30 01:56・rep 0156)**: 正本 v4.0 ec5a3ade・v3.0 再読 2e763fba・vocab_digest 8bc9a651 が dkb と台帳に一致・書き出し一致・受1〜受8・札 全件一致。残り = fed の正本照合 → dkb 締め(LOCKS 解放)→ 受入測定 dec。

**決140〜142(2026-09-30)**: 判定基準 (iii)・札 372 を辞書へ(338ea0b1・vocab_digest 8bc9a651)・v4.0 を演習(一時 DB)→ 正本 10190 へ投入(束 ec5a3ade534840ee)。fed: t16_crosscheck.py で演習と正本を読み取り照合(digest・札 3,554・否定例 3 とも一致)・test_vocab_digest の参考値を仕様から再計算・凍結試験の DB 再生元を正本の記録 r3(000f77df6bdd93b4)へ。残り: bkb の受入(受K0〜K9・|L|)→ 既定 v2・表示 0.3.10・EC2 反映(別 dec)。

**決142 の締め(2026-09-30 01:58・rep 0158・main 593fa069)**: bkb と fed が正本 v4.0 を照合し、3 実装一致。fed は凍結試験を正本の r3(000f77df)に切り替えた。LOCKS は解放済み。dkb の手番は無し。次は別 dec(bkb の受入の測定・fed の既定 v2 と EC2・共通表の改訂)と、次の巡の追加の札付け 114 件。

**fed 正本照合 合格(2026-09-30 01:56・rep 0156 to dkb・main d30503c8)**: ec5a3ade 一致・札 3,554 一致・field_missing 拒否・凍結試験の再生元を正本 r3 000f77df6bdd93b4 に(206/206)・LOCKS 解放。**決142 は 3 実装照合まで完了**。次: 受入測定 dec(bkb・順 5)→ 既定 v2/0.3.10/EC2 dec。

**決142 完了・決143(2026-09-30 01:58〜59・main 38819be0)**: dkb 締め rep 0158(正本 v4.0 ord 2・3 実装 ec5a3ade・LOCKS 0)。bkb が Q-guide 4(26a3b2741ffec4b8)・Q-syn 16(06a6c0940da02639・make_facets_v2_questions.py・S1〜S6 3/3/3/2/2/3)を固定・受入設計 改訂 10 266effe7aa2eb813。**決143**: 受入の測定の実走を承認(bkb・v1/v2 別プロセス・B 9890 読み取り・語彙 10190 v4.0・固定集合 7・受K3 で合否・LLM なし・EC2 不変)。次: bkb の結果 rep → 合格なら 既定 v2・0.3.10・EC2 dec / 保留なら保留事例の利用者判断。coord は共通表 §1-15 改訂案を受入の前に。

**共通表 改訂 13 草案(coord・2026-09-30 02:05)**: §1-19 新設(1-19-1 粒度の札・1-19-2 候補規則 v2・1-19-3 応答の加法・1-19-4 受入・1-19-5 現行版・1-19-6 決85〜決143 一覧 + 番号なし dec 12 本)・見出し行を「改訂 13 草案(承認待ち)」に・§1-1〜§1-18 と §2 以降は不変(機械確認)。次の改訂の項目 76〜78。rev へ短い確認 req 0205 → 合格なら利用者へ確定(決NNN)・確定時に見出し行を書き換え sha 記録。

(改訂 13 草案の確定 sha = 2cf0f129ba925cde・948 行・main acc2a9f2・bkb 受入設計は改訂 11 6601e5ca3106f41b を正本に。rev req 0205 の回答待ち。)

(改訂 13 草案は fed rep 0207・dkb rep 0207 の写し確認(値の誤りなし・札の名を段ごとに分ける 2 点)を反映し sha 36c0d159fc4fcdfe・main f9c52ae8。req 0205 追補 1・2。rev の回答待ち。)

**受入の測定 結果(bkb・2026-09-30 02:10・rep 0210・run_20260930-0204)**: 受K3 = 判定保留(失った事故 延べ 4・異なり 3・説明不能な利得 0・説明済み利得 延べ 539)。型 ① Q-loo strict 2 件 = 個体(断線検出器 8R・32イT軌道回路)を親へ畳み strict の親の組に子の表層が無い → grouped には在る。② Q-syn 問 7 strict/grouped = 親なし個体(下り場内1RA信号機・no_parent)を外した結果(決136 の 2)→ same_equipment には残る。他は全合格(736 問・受K1 0・受K7 683 問 0・受K8 増 0(減 41・0 件の組 2,419→2,309)・受K5 変化 1 升・受K6 4 通り・K4 d1 0・K9 52/52)。判定器 2b0ce06a07724ffd・受入設計 改訂 12 c4b58e2b50061a41・bkb の読み誤り 2 つ(注記の範囲・min_len)は合否の式を変えず是正。証跡 git 外 run_20260930-0204(k3_holds d85a9da2)。d2・戻した B は範囲外。共通表草案 c429ff237a5a3432 に版を反映。→ 利用者へ保留 4 件を諮る。

**決144(2026-09-30 02:14・main d94c2d59)= 保留 4 件は意図どおり・受入合格**(受K3 の式は不変・個別判断の記録)。利用者「手順書を起こす」→ coord が req: fed = EC2 反映手順書(kg_api I0+v2 既定・付録 B の順 0〜7・辞書 JSON の扱い・Dify 0.3.10 包み/DSL 依存/画面・ガイド加法)/ dkb = EC2 語彙 DB v4.0 投入手順書(dump・積荷・registry vocab_ec2・照合・戻し)。手順書 → rev 短い確認 → 実行承認 dec → coord 置換方式で実行(LOCKS・info・退避タグ)。共通表 改訂 13 草案 c429ff237a5a3432 は rev 確認待ち・改訂 14 で受入結果と現行版。

**EC2 反映の手順書 2 本(2026-09-30 02:21・main d334a21a)**: fed `docs/手順_EC2反映_facets-v2_fed_20260930.md` 改訂 0 f7d4888b2f175c28(順 0〜7・退避→読み手 I0 既定 v1(T0-ec2)→語彙 DB v4.0(dkb)→台帳 vocab_digest 8bc9a651→辞書 JSON 338ea0b1 置換+再起動(T0b-ec2)→facets_rules.json で既定 v2→表示 0.3.10→タグ・確認 4 問(四日市 8→6 組・新城 34 号 fold・174TR 16→12・12RUR instance_only)+特異点 3 問・vocab_ref の vocab_digest は台帳の値・**要承認 P1 = 設定ファイル facets_rules.json で旗を読む実装**(置換方式では環境変数不可)・付録 C ガイド文案)/ dkb `docs/手順_EC2反映_語彙DB_v4.0_dkb_20260930.md` 68e910fef5e1592a(複製方式 = ローカル v4.0 dump を EC2 neo4j-vocab に load・前提 C1 全件 hash 6de974dc 一致・D2 ローカル演習・C3〜K1 再起動なし・K2 照合は coord が記録を渡す)。rev へ req 0222(2 本 + P1 の見立て)。合格/条件付き → 実行承認 dec(P1 込み)→ coord 実行(LOCKS・info・退避タグ)。

(fed 手順書は改訂 1 5ebba801afc94fc7 に = 段の名を dkb に揃え(F1・C0〜C4・R1・R2・K1・K2・F2)・K2 fed 分 = coord が t16_crosscheck.py --record(c0f8d4d99cc2cd04)で記録を取り fed が --replay。rev req 0222 追補 1・main 4d2dc3fa。)

(dkb 手順書も改訂 1 3500d15dbdf75503(§5 (b) のみ)。rev req 0222 の対象 = fed 5ebba801 + dkb 3500d15d・追補 2。rev の 2 件(0205 共通表草案 c429ff23・0222 手順書 2 本)待ち。)

**rev 0232 ×2(2026-09-30 02:32)と決145(02:4x)**: 共通表 改訂 13 草案 = 条件付き C13-1(instance_only は H≠∅∧C_cls=∅・優先順に 3 番目挿入)・C13-2(昇順は D-KB だけ)・C13-3(番号なし dec は別経路の承認を含む)→ 反映済み。EC2 手順書 = 要是正 EC2-1(高・決144 の前提「same_equipment に残る」が誤り → coord が保存応答で確認: 前後とも skipped/missing_elements・事故はどの組にも無い)・EC2-2(旧タグ起動は DB 復元後に限る)・EC2-3(C2 と F1 の先後 → coord 決め: C2 は F1 の後 1 回)・EC2-4(C4 の E4 は R1 後)。**決145**: 親の無い個体はクラスの軸に個体のまま残す(決136 の 2 を変更)・受入合格(決144)取り消し・fed 計画 改訂 5 + 実装 + 手順書 改訂 2・bkb 受入設計 改訂 13・dkb 手順書 改訂 2 → rev → 再測定 dec → EC2 dec。instance_only は到達しなくなる。共通表草案は 1-19-2 を更新(確定は再受入後)。

(dkb 手順書 改訂 2 c823778958ffe294・2026-09-30 02:52: 順 C0→F1→C1→C2→C3→C4(E1/E2/E3/E5+旧版再読)→R1→R1'(E4)→R2→K1→K2→F2。fed 改訂 5/実装/手順書 改訂 2・bkb 改訂 13 待ち → rev。)

(bkb 受入設計 改訂 13 86922bddc6201012・判定器 0fc7e4b87860de10(ORPHAN=keep・55/55・旧 v2 応答で 16 問 K1-cls 落ち = 陰性対照)・2026-09-30 02:52。設計の型番号: S2 = 畳み先あり個体・S3 = 有効な畳み先の無い個体(残る)。fed 改訂 5/実装/手順書 改訂 2 待ち → rev。)

**決145(2026-09-30 02:50)**: 決144(受入合格)の前提の誤り(rev EC2-1: 親なし個体を外すと、番号の無い質問では同じ設備の組も skipped で事故を失う)→ 利用者が「親の無い個体はクラスの軸に個体のまま残す」に変更・受入は再測定。fed: 計画 改訂 5(6dec5ec5)・facet_backend aba2454c(1 か所)・試験 37/37 否定例 18・instance_only は到達しない(契約に残す)・手順書 改訂 2(a1da7e5d・EC2-2〜4)。教訓: 「外しても同じ設備の軸で拾える」は番号が無いと成り立たない — 別の軸で拾える前提は、その軸が実行される条件まで確かめる。

**決145 の改訂 3 者分が揃い rev へ(2026-09-30 03:04・req 0304・main 5c521304)**: fed 計画 改訂 5 6dec5ec5c75df9f4(親なし個体は個体のまま残す・instance_without_class_parent = 畳めずに残した・instance_only は到達しないが契約に残す)・実装 facet_backend.py aba2454c8a07457b(1 か所・既定 v1 byte 同一)・試験 c1f6136b 37/37 否定例 18・fed 手順書 改訂 2 a1da7e5dc0c1f015(EC2-2〜4・174TR 16 組のまま・12RUR 個体のまま)・dkb 手順書 改訂 2 c8237789・bkb 改訂 13 86922bdd・判定器 0fc7e4b8。次: rev 合格 → 再測定 dec → EC2 dec(P1 込み)→ 共通表 改訂 13 確定(草案 47004c961fe6b156)。

**rev rep 0316(2026-09-30 03:16・条件付き)**: 決145 の保持規則は fed/bkb で一致・EC2-2〜4 是正確認・確認 4 問整合。条件 = R145-1(bkb・instance_only の出現を直接検出する検査が判定器に無い)・R145-2(bkb・S5 の期待が旧規則)→ req 0325(再測定前)。R145-3(coord・共通表の C_cls 式に保持項・instance_only は契約に残す確定・参照 sha 更新)→ 反映済み(草案 5633400d5392b255)。低 3 点 → fed/dkb info 0325。次: bkb 改訂 14 → 再測定 dec → EC2 dec(P1)→ 共通表 確定。

(低 3 点 反映済み・2026-09-30 03:26: dkb 手順書 改訂 3 845e88b7acb90b1e(実行順 1 本)・fed 手順書 改訂 2 追補 1 541f583bab8f1e64(R1'・dkb 参照・「親が無い」= fold_stop 7 理由)。実行承認の dec ではこの 2 版を引く。残り = bkb R145-1/2(改訂 14・判定器)→ 再測定 dec。)

**R145-1/2 閉鎖・決146(2026-09-30 03:27〜30・main 28355977)**: bkb 受入設計 改訂 14 3879cad1c56b0c44・判定器 4231cd593ec99a1a(58/58・instance_only を全組走査し K1 不一致・SKIP_OK から除外・S5 = 畳み先無ければ個体自身が残る)。**決146**: 再測定の実走を承認(決143 と同条件・前回 run は基準にしない)。次: bkb の結果 → 保留なら保存応答で所在確認 → 合格なら EC2 実行 dec(P1・fed 手順書 541f583bab8f1e64・dkb 手順書 845e88b7acb90b1e)→ 共通表 改訂 13 確定(草案 5633400d5392b255)。

**再受入 合格・決147(2026-09-30 03:36〜)**: run_20260930-0331(736 問)受K3 保留 2 件 = 前回①と同じ事故(Q-loo 445 断線検出器 8R → 9f814777・520 32イT軌道回路 → eq:軌道回路)。coord が保存応答の results.accident_id で所在を確認: v2 の equipment_class/grouped の畳み先の組に在り strict だけで外れる。②は解消。他は全合格(K1 0・K7 683 問 0・K8 増 0 減 25・K5 1 升・K6 4 通り・v1 は前回と全問同じ)。利用者「合格」→ 決147。次: EC2 実行承認(P1 込み)と共通表 改訂 13 確定を諮る。

**決148・決149(2026-09-30 03:42)**: 共通表 第 3 版 改訂 13 確定 bc4f0cec5003f4a9(948 行・草案 5633400d との差は見出し行のみ)。EC2 反映の実行承認: P1(fed・facets_rules.json で旗・環境変数→ファイル→既定 v1・起動時 1 回・不正は停止・req 0342)→ coord が C0 → F1 → C1 → C2 → C3 → C4 → R1 → R1' → R2 → K1 → K2 → F2 → 表示側(手順書 fed 541f583bab8f1e64・dkb 845e88b7acb90b1e)。C1 の前提 = EC2 全件 hash 6de974dc 一致。戻しは逆順・旧タグ起動は DB 復元後のみ。改訂 14 = 決147 + EC2 現行版。教訓: ls|grep の slug が過去ファイルにも当たり N が 2 値になって commit が止まった(fixed by explicit N)。

**EC2 反映(決149・2026-09-30 03:42 承認)**:
- 手順書は dkb 改訂 3(845e88b7)。順は C0→F1→C1→C2→C3→C4→R1→R1'→R2→K1→K2→F2。
- 決149 の表に D1・D2 が抜けていたので指摘し、coord が追補 1 で直した。
- D1 済み: ec2_v40_dump_20260930/neo4j.dump(sha256 493c66a5…3fb9・17.0 秒)。
- D2 済み: E1〜E3 一致・v3.0 の読み直し 2e763fba。
- dkb の残りの手番は、coord に呼ばれてから R1・R1'(トンネル 19892・--register-digest --targets vocab --env ec2 → E4 --release v4.0)と K2。C4 の照合の元は before_v4.0_E1E3.json。
- 注: vocab_copy_check --compare に dump_record.json は渡せない(before を切り出す)。

(決149 追補 1: D1(ローカル正本 v4.0 の dump)・D2(複製の演習)を dkb が先に行う。C3 で load するのは D1 の v4.0 dump・pre_v40_dump 86489139 は v3.0 なので使わない。coord は D1/D2 完了 + P1 rep を確認してから C0。)

(D1/D2 完了・2026-09-30 03:45: C3 用 dump = 本体 knowledge_kb_v8/data/vocab/ec2_v40_dump_20260930/neo4j.dump・sha256 493c66a55370188e1edc04bd5e7b2bbacba2dc0f69b62a57a5b537d3a7433fb9・3,232,611 バイト(coord 再計算一致)・v4.0 ec5a3ade/9e36fa44・停止 17.0 秒。D2 E1〜E3 一致・v3.0 再読 2e763fba。C4 の元 = before_v4.0_E1E3.json。残り = fed P1 rep → C0。)

**EC2 反映 進行中(coord・2026-09-30 03:53〜04:05・main まで)**: C0 退避タグ pre-v2-20260930(9c69dd9954e8)・退避 ~/kgbak_20260930_v2/・反映前応答 pre_c0(a5e1e5d7)。F1 合格(置換 de03737e/8cb6f159・再起動 27 秒・T0-ec2 0 差・v1 default)。C1 合格(v3.0・6de974dc)。C2: 1 回目 /dumps 権限で失敗(停止 38 秒・DB 無変更)→ chmod 777 で成功 f89695ef…(2,151,969 B・停止 41 秒)・前後一致。C3 load 493c66a5 → vocab_data(停止 40 秒)。C4 合格(ec5a3ade/9e36fa44/VR2/VC3554・v3.0 再読 2e763fba)。R1 台帳 vocab_ec2 のみ v4.0(coord がトンネル越し・dec 20260930-0342)。R1' E4 RELEASE_EXISTS。R2 registry 7ed5f291。K1 合格(辞書 338ea0b1・再起動 27 秒・meta v4.0/8bc9a651/ec5a3ade/ready/v1・T0b-ec2 0 差・特異点 strong/medium/strong 不変)。K2 記録 fed_raw_ec2_v4.0.json 10561989(18 本)・ec2_bundle_v4.0 7e8beded・snap_after 6003d7f2 → req 0404(fed/bkb/dkb)。**注意: kg_api の healthz は無く /openapi.json で生存確認・kg_api は 172.31.4.27:8600・diagnose の特異点は results.singularities**。次: 3 者合格 → F2(facets_rules.json 7e2e5161 → 再起動 → 確認 4 問・特異点 3 問)→ 表示側(0.3.10 梱包・swap・DSL 依存・画面 2 つ)→ 締め(タグ v2-20260930・ARTIFACTS・LOCKS 解放・info・利用者の目視)。トンネル 19892 は ssh -f で起動中(終了時に kill)。

**決149 EC2 反映 完了(coord・2026-09-30 04:1x)**: K2 3 者合格 → F2 合格(facets_rules.json 7e2e5161・再起動 19 秒・v2 file・4 問 PASS・T7a 30/30・特異点不変)→ 順 6: プラグイン 0.3.10 pkg 080f8b13(~/plug_v2)・swap → 識別子 jrtokai/jrtokai_kb:0.3.10@9d273dd2e8c77f686d81af57a201f62993c672feabe71811482b681869a9c306・DSL 2 app 自動書換え(枝 A)・smoke 3,772 B byte 同一・画面 shirei 76e31d51/v10 1bfd1fad(docker cp + restart・health 200)・タグ v2-20260930(kg_api 0e4b50cb)/v2label-20260930/pre-v2-20260930。LOCKS 解放・ARTIFACTS 登録・トンネル閉。Dify app id: D 32ab7de6-4481-4acd-878f-9484fd282846・G 1b30f6b5-2632-490b-a9b5-0e42e01f252d。残り: fed DSL 正本更新(req 0415)・fed P3 API 仕様書・coord ガイド v2 加法(付録 C)・共通表 改訂 14・利用者の目視・次の巡(追加札付け 114・顧客確認シート粒度列)。

**決149 の EC2 反映は完了(2026-09-30 04:1x)**: EC2 の語彙 DB は v4.0(ec5a3ade・9e36fa44)、台帳 vocab_ec2 も v4.0、kg_api は既定 v2(タグ v2-20260930)。戻しの土台は C2 の dump f89695ef…(EC2 ~/kgbak_20260930_v2/vocab_dump_pre_v40/)。K2 で dkb の分は合格し、EC2 の書き出しはローカルとバイト単位で同じだった。次は共通表 改訂 14(coord)と、次の巡の粒度列(顧客確認シート)と追加の札付け 114 件。

**反映後の後始末(2026-09-30 04:18〜)**: fed DSL 正本 2 本を 0.3.10 に(3c082fb8/e6bad6c4・rep 0418)。coord ガイド §1-9 候補規則 v2 追補(md a425dc7acc09ebc3・HTML 01afaab7・Artifact PGyZijY3yrfLWFNHTwbS5V 版 4)。fed req 0419(P3 入力)→ coord 採取 v2_resp 33 件(index 660f9bbb)+ openapi_ec2_20260930_v2.json(140cd014)(rep 0421)。共通表 改訂 14 草案 → rev req(確定は rev 後)。残り: fed P3・利用者の目視・次の巡(114 件・粒度列)・反映後の受入(戻した DB・d2)。

**決147〜149(2026-09-30)で EC2 反映まで完了**: 再受入合格・共通表 改訂 13 確定(bc4f0cec)・EC2 は既定 v2(設定ファイル facets_rules.json・P1 = facet_backend de03737e・環境変数 > ファイル > v1・起動時 1 回)・語彙 DB v4.0(ec5a3ade・K2 3 者一致)・プラグイン 0.3.10@9d273dd2(DSL 正本は dify_plan_a の KB_NEW だけ更新・KB_OLD は基準の 0.3.8 と照合する値なので据え置き)。P3: API 完全仕様書 20260930 版(ab64a980・§6 新設・§5 は段 1 の抽出に固定・試験 18/18)。教訓: 札の名に数字が入ると `[A-Za-z_]+` の正規表現が数え落とす(reader の D5 と仕様書の仕5 で 2 回)— 閉じた一覧を正規表現で読む試験は、札の名の字の範囲を新しい札で確かめる。

(fed P3 完了 2026-09-30 04:24: API 完全仕様書 docs/仕様_API_v1_facets-v4_完全版_20260930.md ab64a980d51c423c・1,053 行・§6 v2・試験 18/18。共通表 改訂 14 草案 86b55f79a9f27f3d(rev req 0423 追補 1)。決149 の後始末は揃った。)

**rev rep 0837(2026-09-30 08:37・条件付き)→ 決150 共通表 改訂 14 確定**: C14-1(残りに完了工程が残る → 完了した経緯/残りに分離)・C14-2(正本参照を bkb 改訂 14 3879cad1 に)・低(現行版 04:25・run_0331 製品版 aba2454c vs EC2 de03737e・既定の現在状態・D1/D2)を反映。教訓: python heredoc の assert 失敗が && 鎖の外に出て、INDEX だけのコミット(8109fd1a)に誤った件名を付けた → 実体は 9ec123c6。**heredoc の python の後ろに続く行は && で繋がらない(EOF の後は別コマンド)。python 内で失敗したら鎖を止めたいときは `python3 - <<'EOF' && …` の形にする。**

**決151(2026-09-30 08:5x)**: 利用者の画面の目視 完了 → 決149(EC2 反映)は目視まで閉じた。共通表 改訂 14 f2d6a6cff314303c(決150)。残り = EC2 反映後の受入(戻した DB・d2)・次の巡(追加札付け 114・顧客確認シート粒度列)。**facets 個体 entry 是正(決134〜決151)は完了。**
