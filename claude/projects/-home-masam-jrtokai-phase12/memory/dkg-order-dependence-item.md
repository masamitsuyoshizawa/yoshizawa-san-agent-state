---
name: dkg-order-dependence-item
description: 新作業(決113・2026-09-27): D-KG(原因探索グラフ・bolt 10090)を読む照会の順の明示。X3r-restored で D-KB 応答が 151/646 変わる(安全区分の印 38 問を含む)。dkb が洗い出し → 計画 → rev → 承認 → 実装。X3r-restored の D-KB は直るまで既知 FAIL
metadata:
  type: project
---

B を読む照会の順の明示(決98〜112)が完了した直後、監査 X3r の新検査(書き出しから戻した DB と応答が同じ)の初回で D-KB が FAIL(151/646・1 位の差 0)。切り分けで原因は D-KG の保存の順(正本の B + 戻した D-KG でも同じ 151 問)。違う欄 = 次に確かめる事項・対処の文 88(推定)・候補の機序 86(`_candidates` の `collect(DISTINCT mech)[..2]`・確定)・安全区分の印 38。B の是正の計画 §9 限定 1 で「D-KG の復元比較は測っていない」としていた所。

**Why:** 安全区分の印は顧客への表示に直接効く。決定論の規則。

**How to apply:** B と同じ型で進める(dkb が D-KG を読む照会を洗い出す: ORDER BY 無しの上位 N・collect・set/dict の反復・浮動小数点の加算 → 鍵の規則は D-KG の照会ごとに決める(合意済みの鍵は変えない・新規は統一の向き・答えの変化を先に数える)→ 受入 = 正本対戻した D-KG・種・陽性対照 → rev → 承認 → 実装 → EC2)。X3r-restored は `--all` に含めない(bkb の限定)。[[storage-order-dependent-responses]] [[same-label-different-meaning]]

**計画 改訂 0(2026-09-27 19:24・c5ea2500f0075033)**: 照会 28 本・順に依る 24・実測で差 5 か所。鍵 = D-KG ノード id 昇順・collect 前に ORDER BY・和は足す前に並べる。試作 646/646・変化 昇順 588(1 位 0・判定器入力 0)・降順 620。**決115 = rev へ(req 2030)・昇順・表示の見本は不要(利用者)・判定 LLM 0・実装承認は rev 後。** dkb の道具の誤り(パスワード先頭「-」・パイプで失敗が隠れた)は所管で直す。

**rev 確認(2026-09-27 20:48)**: 要是正 2(中)。DKGO-1 = G2/G19/G18 の鍵が返り値全体の同点を解消しない・Node.id 一意制約は Node ラベルだけ(全ノードの id 非 NULL・複合鍵・辺の多重度は未保証)→ G ごとに鍵か前提+停止。DKGO-2 = 偽 session の最終行反転は Python 側にしか効かず、Cypher 側は最小の合成グラフで確認。所見 = run_query は B 接続(diagnose から呼ばれる)・O3 は judge の候補列 566 不変なら省略可。決116 = dkb 改訂 1。

**改訂 1(2026-09-27 21:02・90411e52456686ee)**: G ごとに出力を区別する列を鍵に(G2 measurement・G18 st.id・G19 は LIMIT を外し Python で 4 列)・前提 P1〜P3 の点検と停止・試験を Python 側/Cypher 側(一時 Neo4j に合成グラフ正順/逆順)に分離・run_query は B 側(D-KG 27 本)・O3 は 566 不変なら省略・2 日。fed 改訂 2 と一緒に rev へ(req)。

**rev 3 回目(2026-09-27 21:09)**: 改訂 1 に要是正 2(中): A-1 P2 の 20 ラベルに ProcedureStep が無く st.id の欠損を見逃す(照会は HAS_STEP 終点のラベル未限定)→ 終点の Node 所属/id を点検し停止。A-2 G1 の鍵 (rank,c.id,k.id,f.id) に detection/prov_* が無く同 rank 多重辺は先勝ち・P3/P4 も止めない → 「同 rank なら返り値も同じ」を許可条件に。決117 = dkb 改訂 2 → rev 4 回目(短く)。

**改訂 2(2026-09-27 21:15・e1c6ff26cf2d8dcf)**: A-1 = P2b(照会がたどる HAS_STEP 終点の :Node と id・本日 0 件・ProcedureStep 119・FlowStep 95)・A-2 = G1 の鍵に detection/prov_doc/prov_ref・同 rank 多重辺 0 組・合成試験と変異。rev 4 回目 req → 合格で実装承認を諮る。

**rev 4 回目 合格(2026-09-27 23:27)→ 決121(利用者・23:34)= 実装 + 受入を承認**(鍵 24 本・P1〜P3・P2b・受入 646/646・種・陰性対照 151・合成試験と変異・X3r-restored PASS 復帰・O3 は候補列 566 不変なら省略・答え 588 問の並び変化・約 2 日・LLM 0)。EC2 反映(O4)は別承認。

**実装・受入 完了(2026-09-28 00:22 dkb)**: dkg_backend a33d7443fff92db2(76b4da6d)・試験 2 本 OK・受入 646/646(旗・種)・陰性対照 170・O2 588・O3 省略(0/566)・X3r-restored PASS 復帰。**決122 = EC2 反映承認 → coord が置換(退避 pre-dkgorder-20260928 → dkgorder-20260928)**。--x3r-restored は契機(コード変更後・投入後・EC2 反映前)で回す。Python 3.12 の sum は補正付きで加算順の依存は実質無い。

**EC2 反映 完了(2026-09-28 00:45 coord)**: dkg_backend a33d7443 を置換(退避 pre-dkgorder-20260928 ec803147 → dkgorder-20260928 94b07d02・旗 ON・diagnose strong)。O4 は dkb へ req。EC2 kg_api の版 = dkg_backend a33d7443・engine c9e8979d・accident_backend 576820f1・federation 959786cd。

**O4 PASS(2026-09-28 00:58 dkb)**: LOO 646 旗 ON/OFF で EC2(a33d7443)とローカル 646/646・EC2 の B/D-KG 不変・宛先の区別を必須。監査台帳に --x3r-restored の行(930a6439・契機 = コード変更後・投入後・EC2 反映前・PASS 3・既知 FAIL 解消)。**D-KG の順の明示(決113〜122)は EC2 まで完了。**
