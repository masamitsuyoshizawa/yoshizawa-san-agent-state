---
name: v3-stage2-s3-plan-state
description: V3 第 2 段 S3(受入の実装)計画 改訂 9 と実装の状態と、承認後にやること・やらないこと
metadata: 
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-09-24T00:07:13.206Z
---

**2026-09-24 時点**: S3 計画 `docs/計画_V3_第2段_S3_受入の実装_bkb_20260924.md` 改訂 2
(`edd33635ea44d578`・310 行)を coord が受領し閉じた(rep 20260924-0856・coord コミット 992ba215)。
3 本(dkb・fed・bkb)がそろい、rev の短い確認(req 20260924-0900・期限目安 2026-09-26)→ 利用者の実装計画承認 が次。
**承認までは道具を書かない。**

改訂 2 の骨子(決35-1・決35-5):
- 実走の順は S4 初回受入(計数→内容照合→digest 独立照合→2 実装照合→交錯・否定の例 1〜7)と
  S6 継続受入(minus-d→候3→対1〜3/凍V-c→投影・別検査 4 鍵→digest 継続照合・否定の例 8〜9)。
- 2 実装照合の入力 = dkb の (a) の投影 JSON(`knowledge_kb_v8/scripts/vocab/`)+ fed の
  `kg_api/kb/scripts/probe_vocab_reader_v3.py` の出力(形は (a) に合わせる)。当方は要素ごとに突き合わせ、
  各 JSON から自分の実装で digest を計算し直す。固定値 `ea443a4d28b7b621`(rev の合成 2 要素)も再現する。
- 待S3-1〜6 は定義待ち 0・実測待ち 6(V:1236/1237〜1247/1259/1269〜1287/1307〜1326/1553〜1578・F2:380〜384/520〜524)。
- 札は 受1〜受8(dkb の 保1〜3 = V §6-3 の式、V の 内1〜16 とは別)。対応表 §3-1-1 を D10 へ渡す(dkb 未確認)。
  受1/受2 は第 1 段の試験計画で rev の指摘番号にも使われている → 段を添える運用で可(coord 決)。
- 待S3-3 の数え直し: `dict_failure_modes.json`(`cbac32d3f50e4039`)の surface_map = 893 行・NFKC 異なり 891・
  equipment 非 null 511・predicate 非 null 807。未解決は S0 後の `dict_equipment.json`(`94885af182efdebd`)で再導出。

**Why:** 承認前に道具を書くと「実装計画の承認」の意味が消える。入力の形は現物が出てから欄名を取る(§0 の 4)。
**How to apply:** 承認の dec が出たら §3 の 6 本を書き、§4 の否定の例 9 つを実走して落ちた理由まで読む。
`comms_unread.py` の [2/6] は re: で判定するので、返信不要の dec/info は注記しても要対応に残る(INBOX 側は注記で判定)。
[[v3-stage1-design-approved]] [[same-label-different-meaning]] [[equality-checks-need-a-timepoint]]

**rev の限定確認(2026-09-24 09:12・決36)**: 是正版 3 本(dkb febee490beeecc48・fed d0a9e8a43920866e・bkb edd33635ea44d578)に要是正 高 1・中 3。S3R2-1 既存束の継続が F では S5-4 送り(B の交4 と不一致)→ 試験側で保持した束で見る / S3R2-2 vocab を必須対象にした初回の空 DB・未登録で G0 が通らない(P:289〜295 の Fail)→ --init-vocab 明示 + DB に公開版 0 件の二重条件で初期化・公開済み未登録は異常 / S3R2-3 受6 が内1・内2 と辺属性のみ → 内1〜7 全体・b_freq 変異 / S3R2-4 2 出力の再 hash は第 3 の投影でない → B:108 から作る。是正 9/25 → coord 確認 → rev 最終確認(4 件のみ)→ 実装計画の承認。

**改訂 3(2026-09-24 09:29・cc351d59722dfa97・411 行・コミット c1e77e49)**: 決36 の S3R2-1/3/4 を反映し coord へ rep 0929。
受入入力は 4 本(辞書 3 本 + failure_predicates.json 560d2970ca84439e — Predicate 正準のみ 10 語は辞書 3 本に行が無い)。
受6 は V 内1〜7 全部(§3-1-2)・変異 2a〜2e は「受6 だけが落ちる」まで見る。第 3 の投影は生成入力から(R1 は台帳から)。
交4 = 保持した束の不変 + 新規停止(fed (F8) と同順)・注入は dkb の試験の枠。
**「定義待ち 0」を撤回し定義待ち 2**(待S3-7 内6 の 16 語の欄 / 待S3-8 束のラベルとノードの区間属性)→ dkb へ req 0929。
教訓: 対応表を一部の項目にしかつないでいないと、「定義を読める」と書けてしまう。全項目をつなぎ、手順を書き下して初めて未定義が出る([[spec-unverified-until-implemented]])。

**改訂 4(2026-09-24 09:37・3c8017d608301ee3・414 行)**: dkb の rep 0934 への追随のみ(検査の中身は不変)。
問3/問4 は本文で確定(辞書 3 本 = vocab-digest-v1 の 3 ファイル・投入は failure_predicates.json を読む・D:267)。
問1(内6 の 16 語の欄)= coord の req 0926 (B)、問2(VocabRelease が束に入るか・ノードの since/until_if_published)= req 0933 (E)(F) の決定待ち。
VocabChange は束に入らない(V:1070)。dkb の読みは契約ではないので、本文に入るまで該当部分は回さない。

**改訂 5(2026-09-24 09:45・ac14067be72aa3a8・419 行)**: 決37(dec 0943)に追随。待S3-7/8 決定済み → 定義待ち 0・実測待ち 8。
(B) Predicate = predicate 常に / b_freq_total・status・note は提案に在るとき / canonical_surfaces は正準に在るとき(順を保つ)/ origin 常に(142/10/6)・無いときは欄を作らない。
(C) 来歴に failure_predicates.json を別欄で足す(vocab_digest は 3 本のまま)。(E) 束は 7 ラベル・9 種(VocabRelease/VocabChange は入らない)。(F) ノードにも since/until_if_published。
残: dkb の設計書の追補版が出たら D:行 と追補版の行へ引き直す。canonical_surfaces に NFKC を当てない読みを追補版で確認。

**決37(2026-09-24 09:43)**: 承認済み設計書 dbd0b3d403a8aa07 の欠け 5 つ(dkb 発見・「行の全欄」を欄名に置き換えて初めて見えた・T2 の 6 欄/HAS_PARENT に続く 3 回目): (B) Predicate 158 の属性 = dkb 案(canonical_surfaces 順保持・同定不使用・origin 常に・無いときは欄を作らない)【利用者承認】/ (C) 来歴に failure_predicates.json 560d2970ca84439e の sha+bytes を 4 本目(vocab_digest は 3 本のまま)【利用者承認】/ (D) V:634 148→158 / (E) 束 = 7 ラベル + 9 型・VocabRelease/VocabChange は入らない / (F) ノードの投影にも since/until_if_published。移行の入力 4 本で dkb・bkb 一致。設計書は追補版へ(dkb 9/25)→ rev 最終確認(4 件 + 追補 5 つ)→ 実装計画の承認。fed 改訂 3 c485a1dc3f01a27e(S3R2-1 = 試験側で保持した束の不変・(F0b) 実体化)。

**決37 追補(2026-09-24 09:50〜09:55)**: (G) canonical_surfaces は原値のまま(NFKC で ﾄﾘｯﾌﾟ が潰れる)/ (H) 内4(7 欄)・内5(5 欄)・内7(名前)の欄名を設計書へ / (I) 来歴 snapshot の欄名 = 並列配列 input_paths/input_sha256(64 桁)/input_bytes・長さ 4・順固定(Neo4j は入れ子 map 不可)。設計書 追補版 2 = 7d9738452825aca7(承認版との差 9 行を機械照合)→ (I) で追補版 3 待ち。fed 改訂 5 c15c825149e48bb5(照合 食い違い 0・(F3c) 締め)・bkb 改訂 5 ac14067be72aa3a8(追補版 3 の後に改訂 6 で D:行/F:行 引き直し)。次 = 追補版 3 → rev 最終確認(4 件 + 追補 G〜I)→ 実装計画の承認。**教訓: 複数宛 dec への 3 者の注記が同時に来ると merge の連鎖で競合マーカーがコミットされうる(0950 で発生・grep で検出し両注記保持で解消)。merge の直後に grep -rl '<<<<<<<' docs/comms を回す。**

**改訂 6(2026-09-24 10:00・6e8d1e0e7d789883・440 行 wc -l)**: V 追補版 3 13a89bc7efe05a13・D f15a5fb7ccd0157f・F c15c825149e48bb5 へ行を一括引き直し(旧版を git から出して行の中身で新版を探す機械照合・41 か所)。
§1-3 に来歴 3 配列(input_paths / input_sha256 64 桁 / input_bytes・長さ 4・順固定・決37 追補 2 (I))の添字照合 (i)〜(v)。否定の例 16 個(2h 追加)。
次: rev の最終確認(4 件 + 追補 G〜I)→ 利用者の実装計画承認。行数は wc -l で書く(改訂 5 で 419 と誤記した)。

**利用者の新依頼(2026-09-24 10:20)= WEB デモ 2 つ + API 完全仕様書**: T0 探索木の画像デモ(段階 2 の前に「見え方」)・B-KB 第 1 段の効果デモ・console/v10/shirei と同じ形の配置・API 最新版完全版の仕様書。決38: 別 URL(B-KB は b-kb-v3.・T0 は t0.)・console で API 全機能・T0 は kg_api に /v1/t0/tree 加法。決39: T0 デモは既存と同じ Basic 認証(段階 1 の「対外に出さない」を緩める・印必須)・効果デモの旧経路は固定 JSON を multi=False で(旧A/旧B の区別注記・S0 後の辞書で旧は 164 号 1 候補)・place_text/line_text は出す。計画: fed 8a5c0f02a7c602b6(効果デモ+仕様書)・af1de04140657252(T0 表示)・dkb 描画 第 4 版 2186cbfaf590022c・fed の req 1032(console・/v1/t0/tree・配置)待ち → 4 本まとめて利用者承認 → 実装。既存 v1 の口 = EC2 /openapi.json(coord が採取 knowledge_kb_v8/data/api/openapi_ec2_20260924.json)。状況報告 Doc = https://claude.ai/code/artifact/4yszB8x2N7QQTLMwy4YrM4。

**改訂 7(2026-09-24 10:55・d53be4a4e6afe52b・501 行)**: 決40(rev 最終確認の新規 2 件)。
S3R3-1: Neo4j はプロパティに null を持てない → 層 A(JSON)/B(DB 物理)/C(独立読取の論理)に分け、2f は層 A で拒否。往復の否定例 往1〜往7。
S3R3-2: 保存属性 4 区分 (i) id+キー (ii) 業務欄 (iii) DB の since/until (iv) 承認済み内部/派生(null 一覧・*_nfkc)。受6 の過不足一致は層 B 全体、論理比較と digest は (i)(ii)(iii 投影)。対a〜対e。
待S3-9(定義待ち): null 一覧の欄名・(iv) の閉じた一覧(dkb 追補版 4 = A7 の追補・実装計画と同じ便で利用者承認)。
教訓: 「過不足なく一致」を業務欄の表だけで書くと、全ノード必須の id/since を余分として落とす。物理層と論理層を分けて書く。

**2026-09-24 10:56 更新**: rev の S3 最終確認(rep 1017)= 前回 4 件閉鎖・新規 2 件(S3R3-1 高: Neo4j はプロパティに null を持てず「欄が在って値が null」は DB 層で欠落と区別できない / S3R3-2 中: 保存属性の集合が id・since/until と衝突)。決40 = 明示 null の欄名一覧を内部属性で持つ(A7 追補・dkb が欄名・候補 null_fields)・復元 3 規則(復1 null 復元 / 復2 拒否 / 復7 一覧と値の矛盾は拒否 null_list_conflict)・一覧の対象は (ii) 業務欄だけ・型別 4 区分 (i)キー (ii)業務 (iii)区間 (iv)承認済み内部、受6 は全体・論理比較と digest は (i)(ii)(iii')。fed 改訂 6 897d0aa64de912fe・bkb 改訂 7 d53be4a4e6afe52b は反映済み。dkb 追補版 4(A7 追補)待ち → coord 現物照合 → rev の 2 件だけの閉鎖確認 → 利用者へ「実装計画 + 設計書追補版 4(A7 追補)」を 1 便で承認(先に別便で諮らない)。期限 9/26。

**2026-09-24 11:02**: dkb 追補版 4 c03b5f4b8fb29f36(§6-3-1-3 A7 追補・null_fields・復1/復2/復7・(iv) 4 一覧・citations_json)+ 第 6 版 c6c34f0bcd6fac62 固定。coord の誤り: 追補 2 で「一覧を持つ型は PhenomenonClaim だけ」と書いたが設備辞書を数えておらず Equipment(parent null 1,807/system 788)も持つ(決40 追補 3 で訂正)。教訓 = 「型ごとに null が在るか」は全入力を数えてから書く。待ち = fed 改訂 7・bkb 改訂 8(追補版 4 の行に引き直し)→ coord 照合 → rev 閉鎖確認(S3R3-1/2 + Equipment null・citations_json・origin 拒否)→ 利用者へ実装計画 + A7 追補を 1 便で。

**改訂 8(2026-09-24 11:02・7383d756b4b938d4・511 行)**: 決40 追補 1〜3 と設計書 追補版 4(c03b5f4b8fb29f36・§6-3-1-3 A7 追補・利用者承認待ち)・D 第 6 版 c6c34f0bcd6fac62・F 改訂 6 897d0aa64de912fe に行をそろえた。
定義待ち 0・実測待ち 9。null_fields(PhenomenonClaim 3 欄・Equipment 2 欄)・(iv) 4 つ・citations_json・往1〜往12・否定の例 18。
2f(欄を作らないべき所に null)と 2i(在るべき欄の欠け)は別の変異。coord/dkb は 2f を欠けと読み「層 B でも拒否」としたが従わず理由を rep 1102 §2 に書いた(V:1419 の括弧の指し違い)。
次: coord 照合 → rev 閉鎖確認 → 利用者へ「実装計画 + 追補版 4」1 便で承認。

**2026-09-24 11:05**: bkb 改訂 8 7383d756b4b938d4・fed 改訂 9 e39e20fc3f772666(追補版 4 の行に引き直し済み)。決40 追補 4 = 追補 3 の「2f を層 B でも拒否」を撤回(2f = null を書く変異は永続化で正常形と同じになり DB 層で検出不可。層 B で拒否できるのは 2i = 在るべき欄の欠け。coord が別の変異を同じ札で扱った)。残り = dkb 追補版 5(V:1419 の括弧 2f→2i だけ・行数 1,926 不変)→ coord 照合 → rev 閉鎖確認 req → 利用者へ実装計画(D 第 6 版 c6c34f0b・F 改訂 9・B 改訂 8)+ A7 追補(§6-3-1-3)を 1 便で。

**改訂 8a(2026-09-24 11:06・a4b16723c7c9b880・512 行)**: 版の記載だけ(V 追補版 5 0834f0450932e3db・D 65f566927a096d25。どちらも 1 行差を diff で確認・行番号不変)。coord は同じ内容を「改訂 9」と呼んで依頼してきた(行き違い)→ 8a で済と返答。決40 追補 4 で 2f/2i の区別は当方の記述どおり採られた。次: rev の閉鎖確認 → 利用者承認。

**2026-09-24 11:07**: rev へ閉鎖確認 req 1107(対象 4 版固定: V 追補版 5 0834f0450932e3db・D 65f566927a096d25・F 改訂 10 2de8297493aa6270・B 改訂 8a a4b16723c7c9b880・範囲 S3R3-1/2 + Equipment null・citations_json・origin 拒否)。合格 → 利用者へ実装計画 + A7 追補を 1 便で承認。

**2026-09-24 12:26 決46 = 実装承認**: rev 閉鎖確認 合格(rep 1138)→ 利用者が S3 実装計画 3 本 + A7 追補(§6-3-1-3)を承認。版固定 V 0834f0450932e3db・D 65f566927a096d25・F 2de8297493aa6270・B a4b16723c7c9b880。順 = dkb W1〜W4 → fed 先行試験 → bkb 2 実装照合 → S4 終了 → S5-4 → S6。軽微 1・2(F 履歴の版名・B の F 改訂 6 参照)は最初の改訂で直し履歴 hash は置換しない。S7(EC2・vocab_files 切替)は別承認。

**決46(2026-09-24 12:27)で S3 実装計画 3 本 + A7 追補を利用者が承認** → 実装に着手。改訂 9(8e53d0b6db289f28・515 行)= rev 軽微 2/3/4(F 改訂 10 へ行の引き直し・2j の作り方・往9/往11 の境界例)。
実装 1: run_v3_bundle_digest.py(cbd2344e440d90d7)自己試験 13 PASS(ea443a4d28b7b621 再現)。
実装 2: v3_vocab_projection.py(a50b5e738addc259)辞書 4 本 → 層 A の投影。型別 16 本が V と一致・保1〜3 一致。
設備の解決の比べ方は NFKC に決着(決49・dkb rep 1239)。当方が旧辞書でも生 450/61・NFKC 444/67 と示し、V:1268/1300 も 444/67 に直った(追補版 8 c90453f54cb13c09)。
順: dkb W1〜W4 → fed 先行試験 → bkb の 2 実装照合(D(a)/F probe の JSON が出てから欄名を取る)。

**2026-09-24 12:45 実装中の契約決定**: 決47 = parent_verified で上へたどった祖先から欄なしの辺で兄弟へ下り直さない(共通表 次の改訂 68)。決48 = dkb W1 完了(neo4j-vocab-v3 10190)・kb_conn CANONICAL に vocab 加法を許可。決49 = 設備の解決の正準名一致は NFKC(194/250/67・MENTIONS 444。V:1271 の 450/61 は最初から生の文字列の値で「旧辞書時点」という coord の説明は誤り・撤回)。現行版 = 設計書 追補版 8 c90453f54cb13c09(承認版 追補版 5 から 4 行差 1258/1268/1300/1416・再承認不要)・dkb S3 計画 1ab541692ac10410・fed 改訂 11 09ff0f9c02ca23f9・bkb 改訂 9 8e53d0b6db289f28。

**改訂 10a(2026-09-24 12:41・bd86368389ea2f52・521 行)**: V = 追補版 8(決46 の追補版 5 と 4 行差・行番号不変)・D/F は決46 の版の行を引く。保2 期待値 194/250/67/444。v3_vocab_projection.py は NFKC 固定(4af364b91ac3495a)。
教訓: 「旧辞書の値だから違う」という説明は、旧辞書で数え直すまで確かめられていなかった([[assumed-current-state-without-checking]])。

**2026-09-24 12:50**: 決50 = R1 の初回移行はノード追加を VocabChange に記録しない(since が担う・R2 以降は op で記録・count 初期値 0)。**決51(利用者承認)= A7 変更: Equipment に source/status/basis を足し 8 属性**(source 3,554・status/basis 対 2,409・無 1,145・片方だけ 0・無いとき欄を作らず片方だけは拒否 entry_status_basis_mismatch。理由 = _entry_display_status が欄なし→confirmed に読み替え、5 属性のままだと proposed 2,409 が confirmed 表示)。現行版 = 設計書 追補版 9 7d4951567d2bf96c(承認版から 5 行差 1147/1258/1268/1300/1416)→ 追補版 10 待ち・fed 改訂 12 3f43dfaae05f415a・bkb 改訂 11 c9168f5899dff004。fed S5-1 完了(facet_backend ce6d537e9b18c2c3・凍結 156/156)。仕様書は e7ea17ee28f532b7(版の出どころ d72229ae)。

**改訂 11〜12(2026-09-24 12:49)**: 決50(R1 の VocabChange 0・vocab_change_count 0・2k)・決51(Equipment 8 属性 = source 常に・status/basis は対・片方だけ entry_status_basis_mismatch・対f/対g・往13・2l)。V = 追補版 10 97baf15301bfa242。改訂 12 = 17e510f3b5d6d06a・533 行。道具: digest edba9efbffdc547d(自己試験 18)・投影 bc26d96011fbe215。否定の例 20・対 7・往復 13。

**2026-09-24 12:55**: 現行版 = 設計書 追補版 10 97baf15301bfa242(承認版から 8 行差 1147/1258/1268/1285/1300/1416/1419/1693)・dkb S3 計画 d259a4324412e515・fed 改訂 13 e5ada4d81da02201・bkb 改訂 12 17e510f3b5d6d06a。fed S5-1〜S5-3(加法の段)完了: facet_backend 6dc9efc7939fac4e・凍結 166/166(coord 別プロセス)・meta 18 鍵(JSON 経路は vocab_release/vocab_graph_digest null)・excluded_surfaces 入れ子。bkb 道具 edba9efb/bc26d960(digest・投影・決51 反映)。dkb 参照実装 knowledge_kb_v8/scripts/vocab/(件数一致)・W2 未着手(LOCKS 待ち)。次 = dkb W2〜W4 → fed S4-F 実走 → bkb 2 実装照合。

**実装 3(2026-09-24 12:52)**: run_v3_migration_check.py(19a5849fae1b7818)= 層 B の照合(受1〜受8・R1 の履歴)。期待 = 辞書→投影→論理から物理への写し。自己試験 21 例で狙った札だけ落ちる。DB は MATCH のみ・接続不可は rc=2。
計画との違い(次の改訂で写す): 否定例 3 は受6 で落ちる(計画は受4/受5 と誤記)・対c は受4/受5。
残り: 実 DB 実走(演習先が出たら LOCKS+info)・往1〜往13 は D(a)/F probe の JSON が出てから比べる道具。

**改訂 13(676e360d54102d48・538 行)**: 札を観測で付け直し(否定例 3 → 受6・5 → 受7 は計画の誤りだった)。
**来歴の照合を追加**: run_v3_migration_check.py 1d0d506e222ab780(自己試験 28)。第 1 段の test_v3accept_vocab_digest.py は S0 後ずっと未検査 rc=2 だった → V:41 の 16 桁で参考値を足し rc=0。

**2026-09-24 13:00 決52**: dkb W2 完了(隔離演習 knowledge_kb_v8/data/eval/rehearsals/20260924-1257_vocab f54aac07・G14/G15/digest/台帳 到達成功・束 digest 2e763fbae25c6a3d・7,840/9,549・release_ord 1・count 0・故障 2 通り期待どおり停止)。共有スクリプト差分 ingest_pipeline +199/−14・rehearse_env +102/−3・kb_conn +2(CANONICAL vocab)。W3(digest)は参照実装で済み。W4 = 正本 語彙 DB への R1 投入へ(条件: bkb へ入力 4 本申告・LOCKS/info・digest 一致・B/D 不変)。未了 = R2 経路(until/since・VocabChange)と否定例 6d/6h/6i/否2-6/否2-7-2〜5 → S4 終了条件。dry-run は --init-vocab 条件 3 を判定不可(本番直前に別読み取り)。

**2026-09-24 13:03 W4 完了**: 正本 語彙 DB neo4j-vocab-v3 に v3.0(release_ord 1)投入・公開・登録。束 digest 2e763fbae25c6a3d(7,840/9,549/要素 17,389)= 演習 = 書き出し独立計算 = bkb 投影。台帳 graph_registry.json に vocab 鍵(count 0・max_release_ord 1)。B f572fce7/D-KG 15935392 不変。書き出し knowledge_kb_v8/data/vocab/bundle_v3.0_r1.json 922baea76b598e2d(ARTIFACTS)。次 = fed S4-F 先行試験(実 DB・読み取り)→ bkb 2 実装照合(--db)→ S4 終了(R2 経路と否定例は dkb が隔離先で並行)→ S5-4 API 組込み → S6。fed S5-1〜S5-5 完了(facet_backend ffcf60e4cf09c942・凍結 169)。

**S4 受入 §5-1 の 1〜3 が全札 PASS(2026-09-24 13:05)**: dkb が正本の語彙 DB(neo4j-vocab-v3・bolt 10190)に v3.0(release_ord 1)を投入(W4)→ 当方が読み取りのみで run_v3_migration_check.py --db。受1〜受8・来歴・台帳・R1 VocabChange 0 すべて PASS。束の digest 2e763fbae25c6a3d…(64 桁)が台帳と一致。読んだ全体の sha256 前後同じ 7dd8938e13395344。
接続: kb_conn の vocab・KB_VOCAB_NEO4J_URI=bolt://localhost:10190・認証は deploy/docker-compose.vocab.yml の NEO4J_AUTH_VOCAB の既定値をサブシェル内で NEO4J_AUTH に渡す(値を出さない)。
残り: §5-1 の 4(2 実装照合 = D(a) bundle_v3.0_r1.json 922baea76b598e2d・F probe・当方の投影を要素ごと)・5(交錯)・否定例の実 DB 実走(隔離先)・往1〜往13。

**改訂 14(412daa1770ddcad8)**: 決53(R2 以降の update 2 形・閉じた辺は新しい辺)。
**2 実装照合の道具** run_v3_bundle_collate.py(be292fe90d33cbaf): 予備で当方 ↔ D(a)(bundle_v3.0_r1.json 922baea76b598e2d)17,389 要素すべて一致・hash し直しも台帳と一致。fed probe 待ち(rc=2)。計画 §3-2 は照合を run_v3_bundle_digest.py に置くと書いていた → 次の改訂で写す。

**2026-09-24 13:15**: 決53 = 失敗版ノードの再採用(第 1 相で since 書換え)・閉じたノードの再開(第 2 相で until 除去・辺は新規)・限定は決31-4 の範囲。決54 = 読み手の札は 2 実装で同じ名(dkb が §6-3-1-3 に閉じた一覧: 既存 5 + entry_status_basis_mismatch + fed の 7 + edge_endpoint_outside)・端点が束に無い辺は読み手が止める(書き手は G15 で両端検査)。fed S4-F 実読取 完了(vocab_db_reader.py 6c9e3d7fbf8818bb・digest 一致・fed_bundle 89e03cd3ce2b8cd1)。bkb 正本受入 全 PASS(rep 1305)・照合道具 run_v3_bundle_collate.py be292fe9・改訂 15 80c75dc58cc53f24。S4 終了 = 3 者照合一致 + 交錯 + dkb R2 経路と否定例。

**3 者照合で一致(2026-09-24 13:16・S4 終了条件の 1 つ目)**: 当方の投影・D(a) 922baea7・fed 89e03cd3 が 17,389 要素すべて一致・片側だけ 0・hash し直した 3 つと台帳が 64 桁で一致。決54 (2) 受7(束) を道具 2 本に足し、正本 DB でも端点が束の外 0。札 14 の名の照合は dkb 追補版 11 の後(往1〜往13)。S4 残り: 交錯試験・dkb の R2 経路と否定例の隔離実走。

**§3-4 実装(2026-09-24 13:20)**: v3_freeze_checks.py 8de8d86c239d31d6 に check_pair_s2 / exclusive_contribution_s2(候3 の門・違うべき 2 欄・not_applicable は vocab_graph_digest と gold_digest の両側 null だけ)。試験 test_v3accept_pair_s2.py 17 件 PASS。第 1 段の check_pair は不変。
交錯試験は dkb の R2 の演習先が出たら dkb と段取り → info。

**2026-09-24 13:25 決55**: dkb R2 経路と S4 否定例 44 件 NG 0(隔離 DB・記録 20260924_vocab_scenarios.json 161ead58)。read committed の実測 → 読み手は P/R 再読で BUNDLE_RACE(fed も実装)。設計書 追補版 11 638f122e3635f4ed(承認版から 12 行差)。札 16 確定。S4 の残り = bkb の交錯試験(6f を dkb の隔離先で独立再現)のみ。dkb の LOCKS 逸脱(+7 −3 を解放後に変更・自己申告)を記録。

**交錯試験(2026-09-24 13:30・S4 の最後)**: run_v3_interleave.py c875b3e064b7f64f。dkb の一時 DB 18691 で、代理の driver/session が VocabRelease の 1 本目の直後に --publish-6f。dkb vocab_db f33cab0a・fed 4bafc97f と b09ce51a がどれも execute_read 2 回・R=2・区間不整合 0・R2 digest 1c3e36f6… 一致。対照の素朴な読み手は R=1 と名乗り中身が R2(値だけの r2 では「R1 とも R2 とも違う」は原理的に出ない → 要素を加える r2 が要るかは coord 判断)。一時 DB は drop、LOCKS 解放。
教訓: 読み手が 1 回の読込みで VocabRelease を 2 回読むので、読み直しの証拠は照会の本数でなく execute_read の回数(fed の注意)。

**2026-09-24 13:31 決57 = S4 終了**: 4 条件(fed 先行試験 b09ce51a・bkb 3 者照合 17,389・dkb R2 経路 44 件 NG 0・bkb 交錯試験 run_v3_interleave.py c875b3e0・3 読み手とも読み直し・R=2・hash = 交錯なし R2 1c3e36f6)。決56 = 読み直し上限 3・BUNDLE_RACE 札 17(設計書 追補版 12 47b3d25645d5ab55・承認版から 12 行差)。次 = S5-4(fed・既定 JSON のまま・DB 経路は KB_VOCAB_NEO4J_*・切替は S7 承認)→ S6(bkb 受入・凍V-c 除く)→ S7(EC2・別承認)。

**S4 終了(決57・2026-09-24 13:31)** → S6 の準備。往1〜往13 の変異の置き場を coord へ req 1332(案 A = dkb の備品に「r1 + 往 k の変異」の口・推し / 案 B = 当方が一時 DB に書く)。§3-6 のため S5-4 の切替の口を fed へ req 1332。答え待ち。

**2026-09-24 13:48 決59 = S5-4 完了・S6 開始**: fed facet_backend 475e2169227cde5b・router 187d3159・読み手 b09ce51a・既定 JSON(KG_VOCAB_SOURCE 未設定で不変)・=db で DB 経路・凍結 186/186・実 DB で DB 経路 = JSON 経路。S6(bkb: 23 例・対の照合・§3-6 切替前後・札 17)→ S7 束 = dkb EC2 語彙 DB 計画 57186286 + fed 切替計画 → rev 短確認 → 利用者承認。決58 = 往復の変異は dkb 備品が生プロパティで注入。dkb 参照実装の是正 origin_field_mismatch(bkb 往12 で発見)。

**S6 開始(決59)・往 23 例すべて PASS(2026-09-24 13:57)**: run_v3_return_cases.py b56b64b1・指定 a61f942d。dkb の備品 --prepare-r1 → (当方が r1 全体を確認)→ --inject-case。両実装とも拒否 14 は期待の札名・値 8 は期待の値。往5 は dkb が "" を返し当方で落ち、fed は registry_mismatch で拒否(どちらも期待内)。往11f は一時 DB の起動失敗で 1 回回し直し。
次: §3-6 = KG_VOCAB_SOURCE json/db を別プロセスで(fed rep 1346: 台帳は KG_VOCAB_REGISTRY か本体の graph_registry.json・DB 経路の vocab_files は 1 件で source/release/release_ord/graph_digest)。B と正本語彙 DB を読み取りで → LOCKS+info。

**2026-09-24 14:00 S6 進行・S7 束を rev へ**: bkb 往1〜往13 23 例 両実装 PASS(rep 1357・往5 は経路分岐・dkb 参照実装は台帳照合なし)。残り §3-6(json/db 別プロセス比較)・§3-4 実走。S7 束 = dkb 第 2 版 ef7b40355bcb1630(認証 = KG_NEO4J_PW と同じ値・compose .env)+ fed 改訂 1 fc95c947852002e5(S7a JSON のまま置換 → S7b 設定ファイル vocab_source.json で切替・案 B の P2 = 資格情報一致・保全タグ pre-s7a-20260924)。rev へ短い確認 req 1359(5 点)。EC2 P2 実測: KG_NEO4J_PW あり・KB_VOCAB_* なし・vocab コンテナなし。承認 = rev 合格 + S6 全合格の後に 1 便(S7a・案 B/A・S7b の 3 点)。

**S6 そろった(2026-09-24 14:03・rep 1403)**: §3-6 run_v3_stage2_projection.py c899f5a1 = KG_VOCAB_SOURCE json/db/db の子 3 つ × 19 問 × 2 回・19 項目 PASS(B 照会 356・結果 433)。往 23 例 PASS。§3-4 は道具と試験+候3 の門(対の実データは凍V-c で範囲外)。限定: dkb 参照実装は台帳照合なし(本番は fed)。次: rev 短い確認 → S7 承認。
教訓: 最初の回は run_query を渡さず結果 0 件の空の一致だった — 一致を数える前に「比べた中身が空でないか」を数える([[hash-match-not-completeness]])。

**S6 合格(決60)→ 改訂 16(8a59ee698f3fd6ec)§5-4**: S7a 後 = 第 1 段 EC2 受入の回し直し / S7b 後 = EC2 語彙 DB をトンネル越し migration_check(vocab_ec2 鍵)+ EC2 API の JSON/DB 応答を §3-6 の投影で比較 / 凍V-c = ローカル・JSON 経路・dkb の minus-d 複製・19 問・check_pair_s2。req 1405(needs_user_approval: yes)で S7 承認の便へ。判断待ち: 由来 d(推し 5 つ全部)・JSON 経路の限定。

**2026-09-24 14:20 決61**: rev の S7 確認(rep 1412)= 条件付き 3 点(S7R-1 設定の優先順位: kb_conn.uri は override 優先で B:100 と不一致・案 B は S7a 前に KG_VOCAB_SOURCE 不在を確認・P2 は実効認証で RETURN 1 / S7R-2 E3 にスキーマ定義一覧・「バイト単位で同じ」撤回 / S7R-3 新設資源の事前照合・削除は記録した資源だけ・kb_conn を含む退避・起動前の容量確認・D の除外根拠)。是正 = dkb 第 3 版・fed 改訂 2 → coord 照合 → rev 3 点閉鎖確認 → 利用者承認(S7a・案 B/A・S7b・凍V-c (b))。S6 合格(決60)。dkb 備品 copy_check 6caa6261/dump 4b3feb56・正本全件 hash 6de974dc(14:06)。

**2026-09-24 14:50 決62 = S7 承認**: rev 閉鎖確認 合格(rep 1445・索引 45 = 属性 34 + 制約所有 9 + LOOKUP 2)。利用者承認 = S7a(第 2 段コードを JSON のまま置換)・案 B(設定ファイル vocab_source.json・事前確認で競合なら案 A)・S7b(EC2 語彙 DB 複製 + 切替)・凍V-c の限定 (b) 認める。固定版 A dkb 第 3 版 4db9e33769419e6d・B fed 改訂 2 0c1f3ca2816cc533・手順書 470a697c6caf5988・道具 d54cee94f0374f00。EC2 事前確認(info 1451): env 4 つ無し・設定ファイル無し・新設資源無し・avail 2,170 MiB・停止基準 起動前 ≥1,800/起動後 ≥800。順 = fed 案 B コード + dkb dump/kb_conn 確定版 → coord S7-A(複製・E1〜E5・台帳 vocab_ec2 は dkb がトンネル越し)→ S7a(bkb 第 1 段受入)→ S7b(bkb 照合)→ 完了 info。rev §3 の申し送り 5 点(E4 は RELEASE_EXISTS 限定・E6・LOOKUP 正規化は名前+作成文・実行は 64 桁・E5 は補助)。

**S7 承認(決62・2026-09-24 14:49)**: 凍V-c の限定 (b) も承認・d は 5 つ全部。合図待ち。段取り(info 1453): S7a 後 = run_v3_ec2_acceptance.py 回し直し + run_v3_ec2_collect.py(4143a0eb・コンテナ内・/tmp に置き後で消す)で JSON 19 問×2 を集める / S7b 後 = 同じ集め手で DB を集めローカルで run_v3_stage2_projection.py --compare-files <S7a> <S7b> -(cf9d01e2)+ run_v3_migration_check.py --db --registry-key vocab_ec2(トンネル)。凍V-c は S7 完了後に dkb へ minus-d 複製 5 本を頼む。19 問の一覧 = run_v3_c44_live.QUESTIONS。

**S7 の接続(coord rep 1455 への答え・2026-09-24)**: (1)〜(3) はコンテナ jrtokai-v9-kg-api の中(docker cp /tmp・コンテナ環境の KG_API_KEYS・後で消す)。(4) 語彙 DB = ssh -L 19892:127.0.0.1:10190 kb-demo-ec2 → KB_VOCAB_NEO4J_URI=bolt://localhost:19892・USER neo4j・PASSWORD は ~/.kb_ec2.env の KB_ACCIDENT_NEO4J_PASSWORD と同じ値(サブシェルで source して KB_VOCAB_NEO4J_PASSWORD に移す・値は出さない)。一致は S7-A 後に coord のコンテナ内の値の sha256 と手元の値の sha256 を比べる(bash -c 'source ~/.kb_ec2.env; printf %s "$KB_ACCIDENT_NEO4J_PASSWORD" | sha256sum')。合図は S7-A の E1〜E5 が通ってから。

**2026-09-24 15:05 S7-A 完了**: EC2 に neo4j-vocab(網 jrtokai-v9_default・127.0.0.1:10190・volume vocab_data・認証 = .env の KG_NEO4J_PW・heap 256m/512m・pagecache 256m)。dump 3cf69f1b… load・E1〜E3・E5 全 24 項目一致(after_ec2_E1E3.json 837226ac…)。退避 ~/s7bak_20260924(kg_api image 026dfd48…・facet_backend 5cf3ebd3・router 6885fc70・kb_conn ceace951)。新設資源 = neo4j-vocab・vocab_data・~/s7_vocab_20260924/neo4j.dump・port 10190。配る物は ~/s7_kgapi_20260924/(dfe4ce39/187d3159/b09ce51a/94f359da/a45a0d67)。次 = S7a(3 本置換・a1〜a5)は dkb の台帳を待たずに可・S7b は dkb の vocab_ec2 + fed の vocab_registry_ec2.json + kb_conn(P3)の後。トンネル 19892 は pkill -f で自分のシェルを殺す事故(exit 144)→ pgrep で除外して kill。

**2026-09-24 15:09 S7a 合格**: EC2 kg_api に facet_backend dfe4ce39・router 187d3159・reader b09ce51a(JSON のまま)・保全タグ pre-s7a-20260924(0c5ac215)/s7a-20260924 = latest(59aa5370)・退避 ~/kgbak_20260924_s7a(旧版 3 本 + 応答 66 件)。a1 33/33(除外 6・除外 5 の構造は grouped/strict → id → dict。初回の比較は list と誤実装し 22 件差 → 直して一致・除外は増やしていない)・a2〜a5 期待どおり。dkb が台帳 vocab_ec2 登録済み(graph_registry.json 19e2b50b…・uri_host localhost:19892)・E4/E5 合格。次 = fed vocab_registry_ec2.json → bkb 第 1 段受入 + JSON 採取 → coord S7b(P1〜P6・kb_conn 94f359da・vocab_source.json・restart・b1〜b7)→ bkb 照合 → 完了 info・LOCKS 解放・旧 v3. の期日(2026-10-08)。

**2026-09-24 15:17 bkb の S7a 後の段 完了(rep 1517・main 8262e45e)**: 第 1 段受入 `--stage2 json` で不合格 0・未検査 3。JSON 採取 = 18 問 200 + 19 問目(120 組)422 ×2(上限 90 の門)・ec2_collect_s7a.json sha256 30c8adab…(git 外 本体 knowledge_kb_v8/data/eval/bkb/s7_20260924/)。ローカル §3-6 JSON と 18 問で投1/投2/meta13 鍵が一致(参考)。道具を直した(コミット 4640fba0): 受入の台 0242cca4(--stage2 json|db・18 鍵を集合で)・shape_checks 186cb2b2・集め手 c33ca0a3(200 以外を refused に)・比べる側 3ad333ba(options/質問/refused の一致を見る・候3 は集めた options に当てる)。次 = S7b の合図 → 集め手で DB 採取 → --compare-files <S7a> <S7b> - --registry-key vocab_ec2・受入 --stage2 db・トンネル 19892 で migration_check(dkb のトンネルが閉じたことを先に確かめる)。
教訓: 計画に「18 鍵」と書いても台の定数は直っていなかった。ローカルで search を直接呼んだ検証は router の門を通らない — HTTP の集め手は実際の口で 1 問試してから本番に回す([[spec-unverified-until-implemented]])。

**2026-09-24 15:20 S7b 合格**: EC2 kg_api を語彙 DB 経路へ切替(案 B・kb_conn 94f359da・vocab_source.json a45a0d67・vocab_registry_ec2.json 04f69852・保全タグ pre-s7b-20260924 11a5e32f / s7b-20260924 = latest 1395e5b3・退避 ~/kgbak_20260924_s7b/resp)。b1 33/33・b2 v3.0/2e763fba…64 桁・b3 vocab_files 1 件 source vocab_db・b4 vocab_digest 不変・b5 snapshot 変・b6/b7 OK。EC2 openapi 不変(33 経路 f377e4a9)。戻し方 = vocab_source.json を消して restart(KG_VOCAB_SOURCE 不在を確認済み)・kb_conn 旧版は ~/kgbak_20260924_s7a/。残り = bkb (3)(4)(DB 応答比較・トンネル照合)→ S7 完了・LOCKS 解放・利用者報告 / fed 仕様書 生成し直し / 凍V-c(bkb・dkb minus-d 5 本)/ 旧 v3. URL を 2026-10-08 に外す。

**2026-09-24 15:23 bkb の S7b 後の段 完了(rep 1523・main d4c00813)**: 受入 --stage2 db 不合格 0・未検査 3。DB 採取 ec2_collect_s7b.json 0fc43028…(18 問 200 + 1 問 422)。S7a と --compare-files vocab_ec2 で 22/22。トンネル 19892 越し migration_check 13/13・束 2e763fba = 台帳 vocab_ec2・読んだ全体の sha256 7dd8938e(S4 正本と同じ → 中身では区別不可・経路で確定)。トンネルは ssh -f -N -o ExitOnForwardFailure=yes で張り、pid を ps で特定して kill(pgrep -f は自分のシェルも数える)。git 外の置き場 = 本体 knowledge_kb_v8/data/eval/bkb/s7_20260924/(7 本)。次 = coord の S7 完了 → dkb へ minus-d 5 本を依頼 → 凍V-c(JSON 経路・check_pair_s2・限定 (b))。

**2026-09-24 15:25 決63 = S7 完了**: EC2 V3 API は語彙 DB v3.0(neo4j-vocab)を読む(案 B・kb_conn 94f359da・vocab_source.json・vocab_registry_ec2.json・タグ s7b-20260924 = latest 1395e5b3)。bkb 独立確認: 受入 db 不合格 0・S7a↔S7b 22/22・トンネル照合 13/13。LOCKS 解放。EC2 の作業物は掃除済み(退避 ~/s7bak_20260924・~/kgbak_20260924_s7a/_s7b・dump ~/s7_vocab_20260924 は残す)。戻し方 = vocab_source.json を消して restart。残り = fed 仕様書(s7b_resp 33 件から)・凍V-c(bkb・dkb の minus_d 5 本 knowledge_kb_v8/data/dict/minus_d/・JSON 経路・限定 (b))・旧 v3. URL 除去 2026-10-08・共通表 次の改訂 54〜69。

**2026-09-24 15:31 凍V-c 完了(rep 1531・main f63d937e)・S7 完了(決63)**: run_v3_freeze_vc.py 8e14f4e7(cbd75811)・7 条件を別プロセス(KG_V2_VOCAB_DIR に一時ディレクトリ・3 本を置き dict_equipment だけ差し替え)・19 問・limit 1000。5 つの d とも対成立。排他的寄与 cooccur 278 / term_index 231 / llm 111 / llm_context 14 / glossary 0。消える切り口は寄与に足さず別計上(cooccur 24・延べ 905 > 278)。対照は対2 で不成立・M 178/178 同じ。ok の切り口は全部 件5/件6 合格・非完全 260 は全部 skipped。dkb の minus-d 5 本は独立検算で一致(info 1529)。結果は git 外 本体 knowledge_kb_v8/data/eval/bkb/freeze_vc_20260924/。残り: 共通表の次の改訂に「消える切り口の数え方」を挙げうる。
教訓: 対照(同じ条件 2 回)を入れると、門が効くこと(対2)と基準の揺れ 0 を同じ回で示せる。

**2026-09-24 15:33 凍V-c 完了**: 5 由来とも対 成立・排他的寄与 cooccur 278・term_index 231・llm 111・llm_context 14・glossary 0(JSON 経路・19 問・limit 1000・full)。消える切り口は別数え(cooccur 24/905・llm_context 66/113・llm 132/70・現れる 18)→ 共通表 次の改訂 項目 70。合否には使わない。第 2 段の残り = 共通表の次の改訂(54〜70)の反映(coord 段取り・承認要)・旧 v3. URL 除去 2026-10-08・仕様書 最終 ac3fdd0b。

**2026-09-24 16:00 共通表 改訂 11 草案**: 決64(利用者が計画承認)→ 草案 44ee838bf56da937(842 行・改訂 10 4d0f3e5d との差 = 書換え 3 行 + 追加: 冒頭段落・§1-16 の 68〜70・決33〜63 の表・§1-8 全5・§1-17 完了状態と現行版)。rev へ短い確認 req 1559(4 点)。合格 → 利用者承認で確定(sha を確定版に・次の改訂の項目の末尾行も更新)。

**2026-09-24 16:25 決66 = 共通表 改訂 11 確定**: 30ba93598a6da687(842 行)。改訂 10 との差 = 書換え 4 行 + 追加(§1-16 決33〜63 の表・68〜70・§1-8 全5・§1-17 完了状態と現行版)。rev 4 点 + 2 点反映(決65)。見出し行の「承認待ち」は承認前の文言(項目 71)。第 2 段の残り = 旧 v3. URL 除去(2026-10-08)のみ。次の改訂の項目 = 70 の続き・71。

**2026-09-24 16:20 決66 = 共通表 第 3 版 改訂 11 確定(30ba93598a6da687・842 行)**: §1-8 全5(消える/現れる切り口は寄与に足さず別計上)・§1-16 決33〜決63・§1-17 完了状態と受入の記録。以後は「§n + 改訂 11 30ba9359」で引く。**第 2 段で bkb の手番は無し**(coord)。項目 70 の足し方の決め方は次の改訂に残る。
