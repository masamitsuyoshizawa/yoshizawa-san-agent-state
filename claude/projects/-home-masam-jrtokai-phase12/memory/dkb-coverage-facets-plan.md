---
name: dkb-coverage-facets-plan
description: 決80(2026-09-25)= D-KB(/v1/dkg/diagnose)の検索カバレッジを facets@v4 の同定と 3 切り口で広げ、location_class / same_equipment を特異点として注意喚起する設計変更の検討(承認まで実装しない)
metadata:
  type: project
---

利用者指示(2026-09-25・原文は dec 20260925-1640): 設計変更は可。D-KB の目的は故障原因探索と対応のアドバイス。検索の基本は equipment_class、location_class / same_equipment が存在すれば重要な特異点として注意喚起する出力を検討する。

現状(確定・コードで確認): D-KB は 3 層辞書 JSON(S0 版 94885af182efdebd・EC2 同版)と症状名の部分一致(最大 3 件)+ 意味照合、設備は辞書で最大 2 件。facets@v4 の語彙 DB・複数意図の全返し・親子展開・3 切り口は未反映(共有はデータ源の血統だけ・同定は 3 実装)。

検討 草案 1 = docs/検討_D-KB検索カバレッジ拡大_facets連携_coord_20260925.md(94d286856b19f674)。案 A 語彙 DB に揃える / 案 B extract を共有し設備 × 現象の組ごとに候補(症状 3 件の上限を外す・LOO700 判定器 v2 基準 294/566 で同一バッチ比較) / 案 C 3 切り口を関数で呼び、same_equipment(強)・location_class(中)を欄 singularities と actions_response 先頭の kind=alert で注意喚起(原文は出さず id・発生日・場所・設備の語・原因コードまで)。順の案 = 段 1(C + A・加法・順位不変)→ 段 2(B)。fed(内部呼出し口・費用)・dkb(症状の対応・上限の由来・LOO・順位不変の道具)・bkb(B 正本での特異点の出現率・出せる欄)に 9/26 期限で検討を req。次 = 改訂 2 → rev 確認 → 利用者の設計承認。

**How to apply:** 効果と劣化に同じ基準(LOO 同一バッチ)・0 件は「無い」と「調べていない」を区別・注意喚起の文面は表示側(Dify プラグイン・画面)で組み立て印は別欄。[[t0-tree-image-demo-plan]] [[loo700-baseline-judge-v2]] [[dkb-recurrence-improvement]]

**改訂 2(65b19fa4c4573b06)と決81(2026-09-25 16:57・利用者決定)**: 実測 = 現象 891 のうち症状へ橋渡し 113(12.7%)・D-KG から候補を作れない問 119/646・症状 3 件の上限は意味照合の抑え(外すと 383 問が超え最大 394)・特異点(647 問・自分を除く)equipment 560 / location 89 / same_equipment 0(番号が在る問 5・LOO では測れない)・場所の規則 facets 159 問 vs D-KB 427 問・照2 の新規 0・上限 90 で 49 問が 422・原因コードは CauseConcept(475/700)。決定: 段 1 = 案 C + A(加法・順位不変)→ 段 2 = 案 D(症状名 102 の対応表・統制語彙)→ 段 3 = 案 B(dkb の形)/ 現象を問わない「同じ設備」の切り口を facets@v4 に足す(共通表改訂)/ 段 1 は facets の場所の規則・踏切は別承認 / **マスク 47 件(place 22・line 30・facets@v4 が EC2 でもマスク前の場所・線区を返す)は判定までマスク側の値を返す(先に・fed 実装・bkb 材料・coord EC2 反映)**。rev 短い確認 req 1651 待ち・fed/dkb/bkb の実装計画 9/26。

**決82(2026-09-25 17:10・利用者承認)**: (1) マスク 47 件の変更を実装承認(fed 計画 db4fbae39eab3448 = facets の RETURN を coalesce + federate F1〜F4 + 効果デモ静的データ 435 か所と仕様書の例 392 か所の作り直し / bkb 計画 f530d79e96c875f5 = search と get_doc の Cypher 4 箇所 + 共通の受入検査 mask_field_check / 駅 5・踏切 4・区間 12 の name_masked。全文側とfederate の証拠本文は 0 件。doc 全文で他の事故の 4 種を含む 2 件は利用者の判定対象に追加。ask は別承認)→ coord が EC2 置換方式(再起動 1 回 + 効果デモ画面作り直し)。(2) D-KB 段 1(dkb 第 4 版 3c9ac6001a9ff854・fed 第 2・3 部)を rev(req 1651 未実施)と並行で承認・旗 DKB_SINGULARITIES 既定 OFF・ON は別承認。(3) same_equipment_any = 要る要素に equipment_class を含む・strong に含む・組の数は案 (i)・most_specific_facet に入れない。alert の固定文言 = 決81 追補 2。再発照合の prov_doc に source_file が出る件はマスク範囲外(0 件)・出典の欄の扱いは別事項。共通表 §1-14 の改訂と B-KB マニュアル 149 行は coord の宿題。

**マスク 47 件の変更は 2026-09-25 17:38 に EC2 反映済み**(kg_api タグ mask47-20260925・facet_backend 917c0989・accident_backend 7c104c43・federation 5bbab9e2・段 8 画面 15da5a23 = 効果デモ dba45157)。受入 = bkb の mask_field_check を EC2 の 4 口の応答に当てて合格(facets は limit 1000 で採ると対象 151・limit 10 では対象 0 で未検査になる)。応答の写しは EC2 ~/mask47/ と scratchpad(値を含む・配らない)。残り = fed の仕様書の例の採り直し・利用者の判定(47+2 件)・fed rep 1731 の (a)(b)(c)。D-KB 段 1 は dkb が旗 OFF で投入済み(dba7a083・646/646 不変・DKB_DICT_ORDER_FREE は A-2 後)。

**段 1 の実装は 2026-09-25 18:04 に完了(EC2 未反映)**: fed facet_backend e86b399e51872740(口 3 つ + same_equipment_any・凍結 200)・dkb dkg_backend eba4694a1d5eedb0(旗 DKB_SINGULARITIES 既定 OFF・LOO 646 で C0/C1 646/646・C2〜C5 合格・A2 646/646・level medium 73 / none 71 / not_checked 502 / strong 0・旗 ON で +14 ms)・verify_singularities eec071bb3146a1d2。決84 = C3 の門は許可一覧の値に収まらない一致 0。共通表 改訂 12 草案 ee40e2158ece8578(4 つ目の切り口 照5・マスク側の値 照6・§1-18 決68〜84)を rev の req 1651 追補 1 に載せた。**次 = rev の確認 → 利用者の別承認(EC2 反映: facet_backend を置くと B-KB 画面に 4 つ目がすぐ出る・kg_api 再起動 1 回・旗 OFF・仕様書の例の採取 18 問 × 2・B-KB マニュアル 149 行)→ 旗 ON は別承認 → Dify プラグインの表示側。**

**決86(2026-09-25 18:35・利用者)**: 判定 = 47 件・名前 21・文字列 8 種はすべて地名・人名 0 → マスク側の値を返す規則を撤回。EC2 は 18:37 に pre-mask47 の版へ戻した(kg_api タグ raw-20260925 = e4c4140c3113・画面は pre-mask47 のイメージ)。fed/bkb は repo の coalesce を外す(段 1 の e86b399e にも入っている・反映前に外す)・試験の期待は生の値に。B 正本の *_masked 欄と Document.text の [氏名] は地名への誤マスク → 正本の是正は別計画・別承認。判定の材料 = 本体ツリー knowledge_kb_v8/data/private/mask47_review_20260925.md(0600・値を含む・配らない)。

**決88(2026-09-25 19:01・利用者)**: 段 1 を EC2 反映(19:03・タグ stage1-20260925・facet 03dc53a6 / router 95a6e1f6 / dkg 1f696b15・旗 OFF・diagnose は既存 29 欄不変 + meta 加法・facets に same_equipment_any)・共通表 改訂 12 確定 db380d60415d50d0・正本の誤マスク是正(dkb 計画 9f9a7e2c)の諮1〜5 承認。残り = fed 仕様書の例採り直し・coord B-KB マニュアル・旗 ON と Dify 表示側(別承認)・正本是正 S1〜(投入は段ごとに承認)・63 件の判定材料。

**決89/決90(2026-09-25 19:20〜19:27)**: 正本是正の判1 = 見本を見て種ごとに決める(dkb が見本を private に)・9 種目は印そのもので戻す文字列は 8 種(一覧 v3 71349dd05709f375)・生の値にも印が残る 4 欄は判定の外・63 件 = 中 5 外 58・bkb 基準値 before_v3 bf7a5a19。表示側(fed 計画 2fbd9bb2)の文言と判1〜3 承認 → fed 実装 → 反映(別承認・Dify uninstall→install)→ 旗 ON(別承認)。fed の仕様書の例は stage1_resp(33 件・openapi 140cd014)で採取済み。

**状態(2026-09-25 19:52・決88〜92)**: 段 1(facets の口 3 つ + same_equipment_any + D-KB の singularities 欄・旗 DKB_SINGULARITIES 既定 0)は EC2 タグ stage1-20260925 に反映済み・共通表 改訂 12 確定 db380d60415d50d0。表示側(fed 1cfda730・Dify プラグイン 0.3.9・指令 UI 151848e74a4804a9・demo_v10 34908d49af59a6c9)も EC2 反映済み(旗 OFF で 1 往復 byte 同一・画面は未目視)。**旗 ON は dkb の設定方法(環境変数は置換方式で変えられない→設定ファイル)の rep の後で別に反映**。B 正本の誤マスク是正 = 判1 を決91 で種ごとに決定(s2/s6/s7 全部・s4 は外の接尾語なし 17 件を除く・戻す 1,245/1,262)→ dkb が位置の表 → S2 設計 → 演習・投入は段ごとに別承認。bkb 受入 受M1〜M6(基準値 before_v3 bf7a5a19d3ec009b)。

**2026-09-25 20:01(決93)**: EC2 kg_api = dkg_backend 06ec226ed29dedeb(B 不通の守り + 旗ファイル)・`/app/kg_api/kb/config/dkb_flags.json` で旗 ON・タグ flag-on-20260925(退避 pre-flag-20260925)。diagnose に singularities 欄が出る(alert の実例は未確認・dkb に質問例を依頼)。S2(除外語)は着手承認・context_free の 1 字 7 項目は判定材料を作る。**docker commit に `-q` は無い。`set -e` はパイプの途中の失敗を止めない(タグを作らずに新版を置いてしまい、戻して作り直した)**。

**2026-09-25 20:36(決94)**: alert の実例 3 問(新城駅構内の34号転てつ器=強・高山構内の12号軌道回路=中・新城構内…不転換=4 つ目の切り口の強)を EC2 の API と Dify で確認。誤マスク是正: S2 完了(氏名リスト 990c2c94・not_names は s4 を除く 7 種で正しい)・1 字 7 項目は c1 接尾語ありだけ 56・c3 全部 91・他は氏名(戻す 147/278)・S3 の前に再マスク禁止。次 = dkb が位置の表に 147 を足し S3 へ → 演習・投入は段ごとに別承認。

**決95(2026-09-25 20:47)**: 誤マスク是正 S3(積荷 unmask・dry-run 1,392+966)と S4(写しでの演習・受M1〜M6)を承認。S5 本番・S6 EC2 は演習の結果を見て別承認。設定 6598ae443e4e8e7f(392/27/8)・位置の表 5e9f72ec + bd8a29a9。

**S3・S4 完了(2026-09-25 21:06 dkb・coord 受領 21:08)**: 積荷 unmask(G16・加法・ac80ead5)。dry-run 文書 1,392・欄 971(双子の漏れで 966 → 971・決91 の表は 323c3ed8 に作り直し)。演習 = 否定例 2(extra/missing で G16 落ちる)+ 正例 rc 0・正本のダイジェスト不変。派生欄は NFKC 作り直し(前に一致を確かめる)。次 = bkb の受入(写し rehearse_conn 20260925-2101)→ S5 を利用者へ。**演習の合否は rehearse_result.json の pipeline_rc と canonical_unchanged、積荷の台帳の unmask_fault / completed で読む**。

**決96(2026-09-25 21:17)**: 演習の受入は受M6 だけ FAIL(範囲外 58 事故の place_text 8 か所)→ 決91/94 の「全部の型」と食い違う古い条件だったので (a) 読み替え(戻すと決めた文字列の外は変わらない)。S5(ローカル B 正本 9890 への投入)承認・S6(EC2)は別承認。教訓: 受入条件は後の決定(範囲の拡大)に追随させないと、決めどおりの変更を FAIL と読む。

**S5 完了(2026-09-25 21:22 dkb)**: ローカル B 正本 9890 へ unmask を commit(rc 0・graph_digest export_graph_apoc 実装 f572fce7 → 360406c6・演習の写しと同値・D-KG/語彙 DB 不変・減った計 2,458)。戻す道 = 書き出し accident_b_20260925-2118(sha256 先頭 01da61b8)。**G0 の潜在不具合**: 語彙 DB の台帳は束のダイジェスト(vocab-bundle-digest-v1)なのに G0 が export_graph_apoc 実装で比べていた(09-24 から)→ e418eb31 で是正。次 = bkb の正本での受入 → S6(EC2 の B)を利用者へ。

**S6 完了(2026-09-25 21:57 coord・決97)**: EC2 の B へ unmask を commit(--env ec2・トンネル 19890/19891/19892・vocab_ec2 の認証は EC2 コンテナの NEO4J_AUTH から shell 変数で渡す・rc 0・digest 360406c6 = ローカル・台帳 b_ec2 更新)。書き出し ec2_s6_20260925/accident_b_20260925-2151(34a6e9a0)。kg_api = eb7d23b9(監査是正版・タグ audit-20260925・旗 ON)。次 = bkb の EC2 受入(req)・dkb の S5 前後の応答差の測定(決97 (2))。dry-run は KB_CONN_MODE=dry が要る。

**EC2 受入 PASS(2026-09-25 22:02 bkb)**: 受M1〜M6・*_masked 0・受M5(doc 50 欄・search 20・facets 21 欄・違い 0)。EC2 の B = bkb 実装 20aaa1e41b9858db = ローカル S5 後。限定 = EC2 投入前の基準値は直接測っていない(推定で同じ)・facets 24 事故は未検査(要約を質問にして 422)・federate 未確認。誤マスク是正は両環境で完了。残 = dkb の応答差の測定(決97 (2))・画面の目視(利用者)。
