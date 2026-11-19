---
name: r1-discriminates-acceptance-bkb
description: 決298〜303 R1(判別の辺 336 本)と direct_causes の bkb の受入 — 実装・是正・表示・投入後まで全部合格(2026-10-24)・以後の基準の写しは r1_commit_20261024
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-23T09:32:27.984Z
---

2026-10-23 dkb req 1828(設計 docs/設計_D-KG_direct_causes_dkb_20261023.md 改訂 1 f5521f6c3459cb2b §5)。bkb rep 1831。

- 期待の作り方: 利得の式(dkg_backend `_next_checks`)は作り直さない。規則 5-1 の文から読み取りの照会で一覧を作り直し、応答は「観測なしでは判別の辺は利得にしか効かない」から導く不変条件で照らす。(i) 入った質問 ⊆ R1 の質問 (ii) R1 以外は rank 以外同じ・相対順保存 (iii) R1 の利得は下がらない (iv) 違う事例は R1 の質問を含む (v) actions の差は next_questions の差の事例だけ。
- 先出し(2026-10-23 18:30・手元 D-KG 10090 digest16 15935392c733b65c): 一覧 = edges.json 6db07bd9857d7f89 と一致(310/26/2/29・名なし 333)・陽性の対照 272/0/18/2・LOO 前後(0007a785 → cbd48869)で (i)〜(iv) 成立・非 R1 1,050 件 rank 以外同じ・b_isolation 224 → 171。
- 接続: run_tree_s1_acceptance.container_pw("neo4j-dkg-v1") + KG_DKG_URI=bolt://localhost:10090、kb_conn は kg_api/kb/scripts。next_questions の項目は q の id を持たず question の文で照らす。
- 次: dkb が G17 を実装・演習の台を作ったら A2〜A7 を回す(dkb に台の保持・写しの sha16・G17 コミット・dkg_backend の版を依頼済み)。
- coord req 1832(決297〜300)へ受入設計 改訂 0 を出した(rep 1842・docs/設計_受入_D-KG_判別の辺R1とdirect_causes_bkb_20261023.md f662a0392ce33f7d・コミット 4dfa1101): 積荷 K1〜K9・影響 I1〜I7・写しのやり直し §3・direct_causes DC1〜DC14(moves_candidates は観測を 1 つ外した診断との比べで照らす・所見の _field_ok は作り直さない)。fed の確認 2 点(req 1814)は dkb の回答待ち。工数 2.75 人日(推定)。決300: ambiguous 2 は足さない・b_isolation 224→171 は受け入れ・正本投入は別の dec。
- 改訂 1(rep 1936・45320014f89df26a・87259831・決300 追補 1 = rev R298-1〜3/D297-1〜4/F297-1〜3 を全部採る): DC6 moves_candidates は**適用前後の点数変化**(反実仮想は使わない・所見は別の印)・木は既存確定後に付加・has_edge 空は no_edge と別の文・公開は key と全パスで名を伏せる(文字列全体を照らす)・DC9 は R284-1 の条件。先出し: prov_doc 310 一致(全 HB)・設備 255 + 16 組 55(頁で先が決まる組あり → coord 判断・記録 r1_dc_20261023/k1b_provdoc_equipment.json 0460a6cf)・判定入力列 D 上位3+related_cases_B 646/646 同一・LOO 3 本 646 id 完全。dkb/fed の改訂 2 待ち → 差分を読み必要なら改訂 2。工数 3.25 人日。
- **dkb の分 合格**(2026-10-23 21:08・rep 2108・dkb-r1 40c16f22・道具 r1_dc_acceptance/r1_dc_impact コミット 90de4529・受入設計 改訂 2 72c299c8): G17 は bkb 専用の一時の台 neo4j-bkb-r1k(18791・札 bkb-r1k・消した)で正例 digest c52f9ef9 一致・負例 12・戻し・変異 4。LOO 646 は dkb と byte 同一。direct_causes は E2 1,636 行・木 74 行・変異 3。表示(DC10〜14)は fed の実装待ち。落とし穴: (1) bkb の既定の環境(語彙 DB の経路)では写しの語彙が failed になり sha がずれる → 語彙の DB の鍵を外して dkb にそろえる (2) 木の項目の kind は q/m (3) 根で答え済みの質問の枝は群を重ねない(同じ答え)/置き換わる(違う答え) (4) 不出の照合を JSON の dumps に当てると「[[]」が印に当たる → 文字列の値だけに。
- 受入設計 改訂 3(rep 2131・3b484391・a2c86e34・決300 追補 6): rev 2101 の 5 点を N1〜N5 に。N1(再開の出どころ)・N2(戻しの照合と削除の間)・N3(木の正規化で落ちる観測の位置)は 40c16f22 で欠陥が起きることを先出しで確認(record_kneg5 a401d378・record_n3 fcd3d670)。I1b は固定の期待 646 id(事例 647 − B の excluded 1・cc5bb973468696fa)。req 2110 は是正の版の再受入まで open。dkb の演習の台は dkb が消した。
- 是正の版 dkb-r1b d81e6a28 の再受入(rep 2224・2026-10-23 22:24): N1・N2・N3・N5・I1b 合格・**N4 不合格**(追補の行が 1 つの設備で所見 2 つ → 後の所見が同じ cid を再追補し owner[cid] が上書き → [偽, 真]・同じ追補が cands_all に 2 回 = 是正前から)。5 点以外は改訂 2 の照合が全部同じ。道具は BKB_R1_TARGET=d81e6a28・記録 r1_dc_20261023/d81e6a28/・足した後の台は bkb の一時の台に after で作る(BKB_AFTER_CONN)。K5 の 2 回目は取り直した digest で先に止まる(理由はどちらでも合格)。
- N4 是正 d0f936a3(owner.setdefault)合格 rep 2231 → 決300 追補 6 の 5 点すべて合格(2026-10-23 22:31)。
- 決301 表示(fed-dc 711caf68・プラグイン 0.3.19 9c0d1e4e・dkg_tree 57a387dd)合格 rep 2239: 道具 dc_display_acceptance.py(手元の口 + 本物のプラグイン・期待は設計 改訂 7 の文から)・記録 dc_display_20261023/。解釈 = 番号なしの「…」は切ったときだけ・機械抽出の括弧は設計と実装で違う(実データに無い)。V2 で伏せた名は B でない辺の行や既存の欄から見える(coord へ確認)。残り = d0f936a3 と組み合わせた版で has_edge 空と adds 真の本文の固定例。落とし穴: 診断の口の応答は {results} の包み・凡例は並び・図の札は引用符つき・request_digest は旗の分だけ値が変わる。
- 受入設計 改訂 4(ab85b416・rep 2244): 正本の投入後の digest は演習の台と一致しない(graph_digest は辺の属性を含み、印の dec・batch が演習と本番で違う・batch を変えると c52f9ef9 → 5b5d3268)。照らすのは (a) 件数 (b) 全辺の比べ (c) 同じ dec・stem で先に出した digest (d) 写し。(c) は dkb が dec と stem を知らせたら `BKB_R1_TARGET=d0f936a3 r1_dc_acceptance.py start → expect --dec <dec の文字列> --stem <stem> → stop` で出す(仮の値 決999/20991231 で 285a5d90c50b8966 が 2 回同じ = 決定的)。
- 改訂 5(43c68afc・rep 2256・決302 = V2 で概念名が見えないことは保証しない / 決300 追補 12 = 投入は件数・集合・写し・戻しの digest + 先出しの digest)。投入の値 = dec「決303」・stem discriminates__commit__YYYYMMDD(予定 20261024)→ 期待 digest16 **27354365273cb606 を正式に固定**(2026-10-24 00:06・rep 0006・投入直前の正本 15935392 を独立に測定)。
- 改訂 6(aca2ebbc・決302 追補 2 = DC13 は伏せた行と本文だけ)。組み合わせた版 main 93a179f3 で DC11 の追いの固定例 2 つ合格(rep 2342・record_fixed2 9d285acf)= 表示の受入は残りなし。残り = 決303 の投入の受入(req 2337・dkb の stem と投入直前 digest を待つ → expect で正式の期待 digest → 投入後 (a)〜(e) → 写しの取り直し)。
- **決303 投入済み・投入後の受入 合格**(2026-10-24 00:32・rep 0032): 正本 D-KG = 27354365273cb606(関係 24,870)・全辺の比べ合格(postgraph)・写し 7 本が r1_baseline / dkb r1_commit と 7/7 一致(MANIFEST dfbd5547)。**以後の基準の写し = r1_commit_20261024**。落とし穴: E2 の safety0/trace1/obsex1 は群の環境変数(DKB_SAFETY_MARK=0・DKB_TRACE=1・DKB_OBS_EXCLUDE_ONLY=1)が要る(base_env が DKB_* を外す)。EC2 は coord。
- 決304 EC2 段 A(2026-10-24): 期待 digest e6fff51a820caeb4(dec 決304・stem _ec2)= 投入後一致・全辺の比べ合格(トンネル 19891 読み取り・--canon-uri)。口の写しは coord が EC2 で取り bkb が照らす(鍵は渡さない)。EC2 kg_api = f889827f の版。期待の写しは版+一部の環境変数だけだと meta(語彙 DB)・singularities・測定法・actions_response が違う(**原因は旗の違い**・データ dir は EC2 と一致 = rep 0115 の推定は誤りで info 0159 で訂正) → R1 が効く欄で判断(1,344/1,345・違う 1 行は C-KG の併発確認)。EC2 の環境の全部(DKB_SINGULARITIES・DKB_RETURN_METHOD・KG_FACETS_CANDIDATE_RULE・語彙 DB)を先に聞くこと。KG_DKG_GRAPH_DIGEST は木の meta だけ。
- **決304 段 B 合格**(2026-10-24 01:40・rep 0140): EC2 kg_api t304(0.3.19)で旗 OFF の 1,345 写しは段 A 後と byte 同一(6c534958)・旗 ON 6 件は手元の期待と 6/6 同じ → 決300〜304 の bkb の受入は全部済み。別件 = info 0116(C-KG 由来の併発確認の 1 行)。
- 教訓(受入設計 改訂 7 §5): EC2 と byte で比べるときは、旗の既定で再生するか、期待を EC2 の環境変数の**全部**で作る。一部の一覧で作ると旗の差が別の原因に見える(rep 0115 で「データ dir の違い」と誤って推定)。
- 決305(プラグイン 0.3.20・凡例の前の空行)合格 rep 0358: 道具 plugin_0320_acceptance.py(決305 の文から期待・1,644 本文)。落とし穴: fed の test_fed_profile は git の履歴を読むので git archive の写しでは PD が落ちる → `git worktree add --detach` の一時の作業木で回し、外す前にデータへのリンクを `unlink "${W:?}/${d:?}"` で外す(rm $W/$d は安全の検査で止まる)。
- 決306 追補 1 F2(答え済みの測定を次の確認から除く・dkb-f2 1f4cce76)合格 rep 0434: 独立の照合 f2_acceptance.py(next_measurements は ms: の後に ns: の追補枠 = 繰り上がりは組ごと)・写し 7/7 = f2_20261024(245cdf97)・変異 2 で --live が落ちる。落とし穴: 変異の版は写し全体を複製する(kg_api だけだと profiles が無く診断が None で error = 守りたい照合で落ちていない)。
- F2 の負例(rep 1115・決306 追補 2): rev R306-3 = bkb の道具も木の old==new を照らす前に飛ばしていた → 是正(全行で答えた測定が無いを照らす)・selftest の合成の負例 7 が全部落ちる。R306-1 = 木の道で「不明」と答えた測定が木の次の確認と項目に残る(診断の口は残らない)を 1f4cce76 で再現(f2_tree_unknown.py)。是正の版が出たら f2_tree_unknown.py・f2_acceptance.py --new <写し>・selftest。教訓: 同じ応答を飛ばす最適化は「直っていないまま同じ」を見逃す。
- F2 是正版 d2e033fb の再受入 合格(rep 1146・2026-10-24 11:46): R306-1 は 3 通りとも残らない・D6 は dkb 設計 改訂 8 の狭い定義(次の測定と次の質問がともに空)にそろえた(bkb は最初「ms: が減った応答」と広く書いた = 相手の定義を先に読む)・写し 7/7(245cdf97)・変異 3(木の answered_extra を外す変異を含む)。
- R306-4a・4b(rev 1208)= bkb の道具にも穴(新しい測定の行の中身・木の新しい項目と診断の対応)→ 照合を足して f2b に合格(rep 1222・f2_acceptance.py b6ceec71)。取り直し不要。木の list_no は種類ごと(質問・測定)の連番。教訓: id の並びだけの照合は、新しく入った行の中身を見逃す(共通の行の比べが効かない)。
- **決307(EC2 に F1 = 0.3.20 と F2 を反映)の bkb 受入 合格**(2026-10-24 13:36・rep 1321/1336): 旗 OFF の前後(t304 6c534958 → t307 7b5c22a6)を f2_acceptance.py --old/--new で照らし、変わった 223 行 = 手元の F2 の差と同じ集合。旗 ON 6 問は t304 と群・件数・候補が同じで、答えた測定 ms:0150 は t307 で次の測定に残らない。

**Why:** 利得を作り直すと実装の写しになり誤りも写る。機構から導く不変条件は独立で、先出しの実測で成立を確かめた。
**How to apply:** dkb の演習の台の知らせが来たら、版を固定して A1〜A8。関連 [[collation-derive-expected-from-source]] [[snapshot-compare-needs-health-and-plan]]
