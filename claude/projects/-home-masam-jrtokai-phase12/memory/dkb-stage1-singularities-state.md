---
name: dkb-stage1-singularities-state
description: D-KB 段 1(特異点の alert・設備辞書の供給口)の実装状態・検の結果・辞書の並び依存の発見(2026-09-25)
metadata:
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-27T09:00:37.893Z
---

2026-09-25 に決82 (2)・決84 で D-KB 段 1 を実装(dkg_backend.py sha16 eba4694a1d5eedb0・コミット 6ee3bd6a・rep 20260925-1758)。旗 DKB_SINGULARITIES・DKB_DICT_ORDER_FREE はどちらも既定 OFF。

- 検の道具は `knowledge_kb_v8/scripts/dkg/verify_singularities.py`(--run / --compare --mode whole|c1|a2 / --summary)。LOO 646 で C0・C1・A2(旗 OFF/ON)は 646/646、C2・C3(決84 の門)・C4 は 0。level は medium 73・none 71・not_checked 502・strong 0。
- 検 C3 は計画の「許容なし」だと 26 事象・61 か所で落ちる(すべて許可一覧の値か型の固定の文字の中)→ 決84 で「許可一覧の値に収まらないもの 0」を門にした。
- **辞書の並びだけで答えが変わる**: dict_equipment.json を逆順にすると 25/646 が変わる(1 位が 3 問)。長さだけで安定整列する 3 か所が原因。DB の並び(id 順・別名は NFKC 順)は今の JSON と同じ結果なので既定は変えない。
- alert の文の型は dec 1657 の原文が**半角の括弧**。私の出力は全角の括弧を半角に変えることがあるので、原文と比べる試験で担保した。
- 検 C5 合格(2026-09-25 rep 1804): bkb の strict 73 問と medium 73 問が要素まで一致。bkb の 79 は grouped も数えていた分。bkb の数えも facets を通すので、facets の照合そのものの独立の確かめではない。
- rev COV-1(決85)を是正(2026-09-25 rep 1817・dkg_backend 1f696b157c7c512f・計画 第 5 版 04bb20e9): 同じ切り口の成功+失敗は partial(件数は下限)・当たり無しで失敗があれば unavailable(none は全成功の 0 件だけ)。実データに partial/error は 0 なので効き目は試験でのみ確認。
- location_class の数は数え方で 85(strict・自分)/73(strict・双子)/89(両組・自分)/79(両組・双子)。
- 決86(2026-09-25 18:35・利用者): マスク 47 件はすべて地名 → マスク側の値を返す規則は撤回。計画 第 6 版 4e6a0208 で記述を撤回。facets が coalesce を外した版が出たら C0〜C5・A2 を回し直す。
- 決86 (4) 正本の是正: bkb へ分担の req 1841(bkb = 8 種の一覧と受入 / dkb = raw_text と masked の整列で棚卸し・除外語・ingest_pipeline・*_masked の扱い)。**[氏名] は Document 530/700・LogEntry 4,632 と広く、多くは本物の氏名 → 8 種の出現だけ戻す**。47 件の外に place_text の [氏名] が 63 件(判定の範囲外)。*_masked は 09-09 apply_place_restore が保全した誤マスクの旧値。
- 決87: 段 1 は実装完了・rev 条件付き合格。撤回版 facets 03dc53a6 で回し直し全合格(rep 1850・C3 厳格 24 事象 56 か所)。表示側は count_complete=false なら「確認できた範囲で少なくとも N 件」と未完了の切り口を出す。
- 正本の是正: 計画案 9f9a7e2c(rep 1845・諮 5 件・承認待ち)。一覧は coord の mask47_allowed_strings(cc1ce08c)だが**諮1(マスク前の原文を読む)の承認まで読まない**(bkb の同種操作が自動判定で止められた)。
- 決88(2026-09-25 19:01): 段 1 を EC2 反映済み(stage1-20260925・旗 OFF)。表示側の計画を fed と作る(別承認)。是正計画の諮1〜5 は推しで承認。
- S1 棚卸し(rep 1910・値は出さない): 703 ファイル並べられた。8 種は文書で 1,262(47 の外 820)・1 字の s2 だけで 770・接尾語つき 13。派生の欄は一意に対応づかない [氏名] が 13,435。識別子にも出現 96。**判1〜3(範囲・派生・識別子)を諮って S2 以降は止めた**。棚卸しの道具は scratchpad(未コミット)。
- 決89: 判1 は利用者が見本を見て種ごとに決める。見本 = private/mask47_samples_by_string_20260925.md(0600・25edd032・一覧 v3 71349dd0 = 印を外した 8 種)。文書に出るのは s2 770・s4 27・s6 132・s7 333。判2 = 記録から・判3 = 識別子は変えない。
- fed の表示側の見本 16 本(eval/dkb/singularity_fixtures_20260925・manifest b663dd1e)と約束(欄の形・_sing_* の名前と引数を変えない)。
- 所見: B に届かないと旗に依らず diagnose が落ちる(_pb_summaries に受けが無い・以前から)。直すか coord と相談中。
- 2026-09-25 19:52 dkg_backend 06ec226e(dabcd409): B 不通でも落ちない守り(記録して uncertainty)・旗は env > kg_api/kb/config/dkb_flags.json > 0(EC2 の置換方式用・リポジトリに置かない)。通常時は前版と 646/646 同じ。
- 決91: s2/s6/s7 全部・s4 は中全部+外は接尾語ありだけ。位置の表 5e9f72ec(文書 1,245・欄 869)。原因 = 4 種が name_scrub の context_free(部分一致の全置換・1 字は字の全出現)。S2 設計済み(計画 改訂 2 0b7a83b8)・着手の合図待ち。context_free の 1 字の項目があと 7(範囲外の所見)。
- 決93(2026-09-25 19:55): EC2 旗 ON 済み(flag-on-20260925)。alert の例 = 強「新城駅構内の34号転てつ器が転換不能です。…」・中「高山構内の12号軌道回路が故障しました。…」(rep 2003・ローカル B で確認)。
- S2 完了(rep 2007): 氏名リスト 6d20d105 → 990c2c94(退避あり)・not_names は s4 を除く 7 種(8 種だと決91 と食い違う設計の矛盾を実装で発見・確認待ち)・name_scrub 3792812568f4e982。**S3 の前に name_scrub の再マスクを回さない**。1 字 7 項目の見本 53363f19(文書 278 か所)を判定材料に。
- 決94(20:36): 7 種の読み替え承認・1 字 c1 は接尾語ありだけ(context_limited へ)・c3 は全部(not_names へ)・他 5 は氏名。氏名リスト 6598ae44(CF 392・CL 27・NN 8)。位置の表 決91 5e9f72ec(文書 1,245・欄 869)+ 決94 bd8a29a9(文書 147・欄 97)= 文書 1,392・欄 966。計画 改訂 3 f94e2135。
- 決95: S3・S4 完了(ac80ead5・rep 2106)。積荷 unmask = G16(unmask_fields.py 2d619cce・欄ごとの前−後=期待)。演習 正例 rc=0(文書 1,392・欄の値 997・*_masked 73 消去・写し 360406c6)・否定例 extra/missing とも G16 で落ちた。**位置の表に双子の事故 2 組の漏れ(ファイル側から回した誤り)→ 決91 表を 323c3ed8 に作り直し・欄 971**。派生の *_norm は NFKC で作り直し。写し(18687/18688)は bkb の受入のため残し LOCKS も保持。
- bkb の写しでの受入(rep 2109): 受M1/1b/2(計 2,379)/4・*_masked 撤去 PASS・受M6 FAIL(外 58 事故の place_text 8 = 決91 の 6 + 決94 の 2・どれも「全部の型を戻す」文字列)→ 受M6 の読み替え (a) を coord に諮った(rep 2110)。写しと接続情報のファイルを消し LOCKS 解放。
- 決96: 受M6 は読み替え (a)・S5 承認。S5 完了(2026-09-25 21:21・rc=0・stem unmask__commit__20260925): B f572fce7 → 360406c6(演習と同値)・台帳 b 更新・戻す道 = accident_b_20260925-2118。**G0 が語彙 DB を別実装で比べる潜在不具合(09-24 4a266a53 から)で 1 回停止 → e418eb31 で是正(登録と同じ束のダイジェスト・method 照合)**。
- 正本での受入 PASS(bkb rep 2133)。監査 FAIL 3 を直した(ed2a2198・dkg_backend eb7d23b9): X1 = 09-14 のコメントが gold 文を引用(23 件すべて)・X3r = 鍵が 1 値の sorted 3 か所・X17 = 既知 2 行のまま + unmask を NA。変更前コードと 646/646 一致(一時 worktree で旧コードを S5 後の B で回して分離)。**S5 で D-KB 応答が OFF 7・ON 8 件変わった(欄は未測定)**。
- 決97: S6(EC2 の B に unmask)完了・EC2 kg_api に dkg_backend eb7d23b9。S5 の効果 = 1 位・順位の変化 0(rep 2210)。X17 unmask 行は測定済み。
- 残り: 保存の順に依る B の照会の洗い出しと順の明示(応答が変わる → 別承認)・EC2 とローカルの応答の一致の確認。
- 2026-09-26 17:23 順の明示の計画 改訂 1(b70c486b9aceba52・rep 1723 × 2)。bkb の AS/AF を合流・3 列(保存/種/加算)・E4(_bayes の set)を新たに発見。**API が読むエンジンは kg_api/kb/scripts 版(82402eb9)で、accident_kb_v7/demo 版(b780c8b3)ではない**(bkb が demo 版を読んでいた)。エンジンの事故 id は既存の昇順を保つ異論を coord が採った(info 1726: 合意済みの鍵は変えない・新たに足す所は id DESC/文字列 ASC/生の得点 DESC)→ 改訂 2 e6edf85ead5b0b27(rep 1727)→ **改訂 3 52e9a6b351497447(rep 1731)= 「本番の API は種が固定されない」の誤りを訂正**(kg_api/app.py 18〜19 行は PYTHONHASHSEED=0 でなければ起動しない・EC2 compose も 0。種の依存は本番では揺れない。鍵を足す理由は別の入口での再現と可読性)。→ rev 再確認 要是正 4(決103)→ **改訂 4 a26f4e307d982609(rep 1825)**: 前提の破れは停止・不合格(監査 FAIL・測る前に止める)・「崩れても順が決まる」は取り下げ・比べる組を段 1(旧対新エンジン・差は E1/E2/E4 の欄に限定)/段 2(fed・両側新エンジン)/段 3(正本対戻した B)。→ **改訂 5 563d23429ad68ebc(rep 1833)= 段 1 の許容欄に E3(repairs の顔ぶれと並び)を足す漏れの是正**(fed 改訂 3 7ad805a8 と一致)。**rev 3 回目(rep 1849)で dkb 改訂 5 は残件なし = 確定版**。残りは fed の費用管理だけ(決104)→ rev 4 回目 → 利用者へ実装承認。dkb はエンジンの是正の単独コミットから(承認の後)。
- 決107(2026-09-26 21:04 利用者承認)で実装: エンジン c068a2b1(82402eb9→c9e8979d・bkb/fed 合意の後に main)・dkg_backend D1〜D9 db171ec4(eb7d23b9→4473522a)・試験 test_engine_order 8/test_dkg_order 6(変異で D2/D3/D6 FAIL 確認)。段 1 PASS(エンジン差 614/646 すべて E1〜E4 の札・D-KB 応答差 0)。戻した B = neo4j-order-b 18692(digest 360406c6・接続は private/order_b_restored_20260926.json)。段 3 PASS(rep 2152): 正本対戻した B 646/646(旗 ON/OFF)・種 0/1/2 で 646/646・旧コード 70/16(陽性)。**段 3 の種の検査で D11(概況照合の線区を set から任意に取る)を発見・規則 (7) で是正 → dkg_backend 9d608475**。**O2 = 是正前後で 375/646・1 位 110(降順の鍵のため。昇順の比較用なら 152/61)・判定発火 240/566(昇順 117)**。coord に (1) 降順のまま O3 で良し悪しを見る推し (2) dkb の O3(約 2.7 ドル)が決107 の範囲かを確認中。戻した B・一時ツリーは片付け・LOCKS 解放済み。記録 = 本体 knowledge_kb_v8/data/eval/order_fix/。
- 決108(利用者): 向きは両版を O3 で判定して選ぶ・判別不能なら昇順。昇順版 = ブランチ dkb-asc 59c55ffb(d3648e30)。O3 結果(rep 2235・3.92 USD): 降順 9/10 判別できない・昇順 5/3(+2・規則上は改善だが p=0.73)→ どちらでも昇順。**決109 で昇順を採用 → 完了(rep 2254)**: main の dkg_backend = 2416fd2836d7d979(54a66c8c・d3648e30 とコメントだけ違う)・試験 8 件+変異 D2/D3/D6/D11・昇順版の段 3 PASS(646/646 ×旗 ON/OFF・種 0/1/2・旧 70/16)・O2 152/61・計画 改訂 6 0c36c4d6。残り = EC2 反映(O4)は fed の評価とそろってから coord が諮る。
- 決112(2026-09-27): EC2 反映済み(coord)。O4 dkb PASS(rep 1725・ローカルのコード × EC2 の B/D-KG トンネル 18890/18891・646/646 旗 ON/OFF・限定 = /v1/dkg/diagnose は exclude_ids も旗も受けないので EC2 の API プロセスは通していない)。X3r の入口 audit_dkb 0e181aac `--x3r-restored`(--all 外・18700〜18703・bkb/fed の段に同じ戻した B)・stage3_dkb 85dbf38c `--old-code`。初回(rep 1800): bkb/fed PASS・**D-KB FAIL 151/646(1 位 0)= D-KG の保存の順**(戻した D-KG だけで同じ 151・機序 collect(DISTINCT mech)[..2] 確定・質問文は推定)。D-KG の順の明示を新しい作業として coord に諮り中(それまで X3r-restored は FAIL のまま)。
- 決113(利用者): D-KG の順の明示を新しい作業に。計画 改訂 0 = docs/計画_D-KGを読む照会の順の明示_dkb_20260927.md c5ea2500(rep 1924)。28 本・順に依る 24(G1〜G24)・実測差 5 か所(dkg_order_trace.py)。試作 ブランチ dkb-dkgorder-proto 76b8a54c(609ea6cb・id 昇順): 正本対戻した D-KG 646/646・O2 588/646(1 位 0・判定入力 0 → 判定不要)・降順 620。rev → 承認待ち。**教訓: 一時コンテナのパスワードに token_urlsafe は約 1.5% で先頭「-」→ 起動せず。パイプで失敗が隠れ両方正本で測った(破棄・取り直し)**。rehearse_env.py・vocab(dkb)は token_hex に直した(5b9a8b42・rep 2035)・rehearse_env_c(ckb)は ckb の判断。決115: 向き昇順・見本不要。rev 要是正 DKGO-1/2(決116)→ **計画 改訂 1 90411e52(rep 2102)**: G ごとに出力を区別する列を鍵に(G2 measurement・G18 st.id・G19 Python で 4 列)・前提 P1〜P3 を数えて破れたら止める(多重の辺 3 型は無害と確認)・試験は Python 側と Cypher 側(一時 Neo4j・合成グラフ正逆・変異)・run_query は B 側。rev 再確認待ち。次 = bkb 改訂 2・fed 改訂がそろったら rev 再確認 → 実装の承認(費用込み)。

**Why:** 次の再開で、何が承認済みで何が測り終わったかを取り違えないため。
**How to apply:** 段 1 の続きは rep 1758 §4 の残りから。旗 ON と EC2 は別承認。関連: [[fed-vs-dkb-comparison]] [[verify-in-separate-process]] [[errors-can-cancel-each-other]]
- 決121(2026-09-27 23:34 利用者): D-KG の順の明示の実装 + 受入。実装 76b4da6d(dkg_backend 2416fd28 → **a33d7443**)・道具 57b550e6(dkg_order_premises b2a7c17e・stage3_dkb 133caf07・audit_dkb a6c9deac)・試験 03beac9d(Cypher 側 合成グラフ正逆・旧 2416fd28 で G ごとに差・変異 8・Python 側・前提の否定例)。受入 PASS: 正本対戻した D-KG 646/646 ON/OFF・種 0/1/2・陰性対照 170・O2 588(1 位 0)・判定入力 0/566 → O3 省略。**発見: Python 3.12 の sum() は補正付き(Neumaier)で足す順の差がまず出ない → G4 は陽性の対照を作れない(ローカル・EC2 とも 3.12)**。**X3r-restored PASS(D-KB 0/646・陽性 223・fedB/bkb PASS)= 既知の FAIL 解消(rep 20260928-0022)**。--x3r-restored は --all に入れず契機(コード変更後・投入後・EC2 反映前)で回すことを提案。決122: EC2 反映済み(coord・dkg_backend a33d7443・タグ dkgorder-20260928)・**O4 PASS(EC2 の B/D-KG とローカルで 646/646 旗 ON/OFF・rep 20260928-0058)**・監査台帳に --x3r-restored の行(930a6439・契機 = コード変更後・投入後・EC2 反映前)。D-KG の順の明示は完了。
