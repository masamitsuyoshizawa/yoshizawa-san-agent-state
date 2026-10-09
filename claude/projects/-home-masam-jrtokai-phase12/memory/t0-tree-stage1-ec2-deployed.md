---
name: t0-tree-stage1-ec2-deployed
description: 探索木 第 1 段(/v1/dkg/tree・Dify の木)は 2026-10-03 に EC2 反映済み(決176/177)— タグ・識別子・道具・残りの所在
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-10-02T19:52:47.957Z
---

探索木 第 1 段は 2026-10-03 04:34〜04:52 JST に EC2 へ反映した(決176 承認 → rev の短い確認 = 可 → 決177 完了・dec 20261003-0452)。

- kg_api: タグ `tree-20261002`(作り直し・env `KG_DKG_GRAPH_DIGEST=15935392c733b65c`(EC2 D-KG・export_graph_apoc の実装・2026-10-02T19:35:06Z)と `_AT` を追加)。旧コンテナ `jrtokai-v9-kg-api-pre-tree-20261002` は 2026-10-09 まで停止で保持。
- 受入 A1〜A6 合格(openapi 差 = /v1/dkg/tree だけ・既存 diagnose 33/33 同一・facets 30 差 0・tree 22 問 200・cold_start → 通常)。A2 の「LOO 層化 30 問」は facets の 30 問で代えた。
- console app.py 291776baf21b0dd2・Dify プラグイン `jrtokai/jrtokai_kb:0.3.11@20ab8875…`・DSL 正本 guided 0548e33a242ec211 / diagnose 06ac1ecbfe490310 を 2 app(D 32ab7de6・G 1b30f6b5)へ import・公開・smoke 6/6。
- 道具: `deploy/kg_api_v2/ec2_recreate_with_env.py`(env 追加で作り直し)・`ec2_tree_snapshot.py`(diag/cmp/tree/edge)・`deploy/dify/tools/ec2_dify_dsl.py`(apps/export/import/publish)。EC2 の作業置き場 `~/treedeploy_20261003/`。
- 残り: 利用者の目視(指令 UI)・bkb の EC2 受入(req 20261003-0452)・空白入力が回答 0 字で終わる件は反映前後の比較が未確認・専門家の安全区分付け(別承認)・共通表 改訂 16。

**Why:** 次に kg_api の env を変えるときも同じ道具で作り直す。プラグインを替えると DSL の依存の識別子は自動で書き換わるが、DSL の中身の変更は import と公開が要る。
**How to apply:** 関連 [[t0-tree-internal-state-contract]] [[dkg-tree-p2-plan-fed]] [[tree-stage1-acceptance-bkb]] [[dify-plugin-update]] [[accident-rawdata-api-item]]

## 追記(2026-10-05): 第 2 段

- 決178: 表示改善の順 = 第 1 項(返信の先頭に「受け取った回答」)→ 第 2 項(質問の見出し・案 C 型の分解・上限 60)→ 第 3 項(経緯の描画)。
- 決179 (1): b_isolation の質問文が図・表に出ていた逸脱を単独で是正し EC2 反映(タグ bfix-20261005・dkg_tree 8c353ff9)。
- 決180/181: 第 1 項を実装・受入・EC2 反映(kg_api `recv-20261005`・dkg_backend 0a317ad7・dkg_tree_state b52aa51f・dkg_tree 11110130・プラグイン `0.3.12@543ec924…`・DSL guided 52f60d52 / diagnose a15cfdaf・計画 P2 改訂 14 4ca04ee9・P1 改訂 11 0deedf8b)。rev は計画を 3 巡(R179-1〜6 → 残件 4 → 条件 C1/C2)。
- 残り: 利用者の目視・第 2 項の実装承認(rev は条件付き可)・別件 3 つ = 口の型 DkgObservation に phase が無い(復旧後正常の除外が HTTP で起きない)/ C の接続失敗が応答に残らない(dkb と bkb の台で C 接続が欠けていて気づかれなかった)/ 木の所要が予算 5 秒に近い。
- **EC2 のホストから Dify の console へは SG の IP 制限(2026-10-05・443 は許可 IP だけ・社内担当者の IPRESTRICT-01)で公開 URL 経由では届かない** → `sitecustomize.py`(DIFY_LOCAL=1 で nip.io の名前解決を 127.0.0.1 に)を PYTHONPATH に置いて操作する(`~/recvdeploy_20261005/`)。利用者の IP が変わると 443 が閉じる(SG sg-03676b20bd23d84ed に /32 を足す・saml2aws が要る)。

## 追記(2026-10-06): 甲乙丁の EC2 反映(決197/198)

- EC2 kg_api タグ `abc-20261006`(退避 `pre-abc-20261006`・2026-10-13 まで): 甲 = C 接続失敗の印 + F1(c_connect 2 秒/3 秒・失敗後 30 秒は :deferred「一時保留」の文)/ 乙 = phase の型(Any = None)・口の正規化・質問の phase の落とし・「(復旧後)」(**規則 8 のプロンプトは 3 回の評価で不合格 → 打ち切り・累計 4.49 USD**)/ 丁 = 意味照合の失敗の印(on_semantic)。プラグイン `0.3.13@95de8dde…`・DSL guided 167d551a / diagnose 90943b2e。
- 手順書 `docs/手順_EC2反映_甲乙丁_coord_20261006.md`。写し `knowledge_kb_v8/data/api/ec2_abc_20261006/`。EC2 の C への健康な接続確認は最大 9.3 ms。
- 第 2 項(見出し)は 10/05 深夜に退行で一度戻し、再現せず 10/06 00:4x に再反映(原因未確認・ホストの記憶の逼迫までが観測・EC2 は available 1.6 GB・swap なし)。
- 残り: 戊(照会の待ちの上限 = サーバーの合図 recv_timeout を下げる案 A が実測で成り立つ・rev 2 巡目中)・丙・第 3 項・技術説明資料の訂正(決196・ckb 照合中)。

## 追記(2026-10-06 夜): 戊己庚辛の EC2 反映(決203/204)と記憶の逼迫

- EC2 kg_api/console タグ `bo-20261006`(退避 `pre-bo-20261006`・10-13 まで): 戊-b(案 D/E・派生表 5 つ・skipped)・戊-c(D-KG:補助:failed・固定文・プラグイン 0.3.14・代替経路の穴)・己(B 準備失敗の試し直し 30 秒)・庚(固定文 A/D-KG/federate 5 口・health)・辛(A 準備失敗の試し直し・BGE-M3 は読まない)・rev 実装1〜3 の是正。bkb EC2 受入 合格(rep 2215)。
- **EC2 の性能低下の原因 = ホストの記憶(16 GB・swap なし・旧デモ neo4j 4 本 ≈ 3.9 GB)**: federate を呼ぶと埋め込みモデルが kg_api に常駐(RssAnon 1.15 → 1.9 GB)→ page cache が追い出され io 待ち → diagnose 0.5 → 8 秒・予算切れ 20/22。**新旧コードで同じ(A/B 実測)**。10/05 の第 2 項の退行も同じ型と推定。復旧 = kg_api restart。soak を測るときは federate を呼ばない。**決205/206(2026-10-06): 旧デモ 7 本(neo4j 4 + streamlit 3)を docker stop(volume 保持・vhost 残置)→ available 5.5 GB・federate 後の soak も正常。「federate 後に restart」の注意は不要に。戻し = docker start。**
- **第 2 部(案 A)完了 2026-10-09 01:30(決207/208)**: 4 DB keep-alive 1m → 5s・probes 2(合図 10 秒)。語彙/D-KG は docker run 作り直し(EC2 ~/planA_20261009/recreate_keepalive.py)・B/C は compose に 2 行。退避 = ~/planA_20261009/。観測 24 時間 → 2026-10-10 に defunct/ServiceUnavailable を数える。EC2 で投入・監査を走らせるときは合図を読む driver(6.1.0)で。
- **表示改善 第 3 段(5 件・決207(B)・209〜214)**: 実装 全部 main(dkb tree_state 60d0171e・router edc6c24a・fed dkg_tree bae905c6・schemas 012fb5cc・プラグイン 0.3.15 bb67b3dd・DSL 835665af/fad57a2f・console f9494c61・shirei a78794f3)・bkb 受入 S 項 全合格(S6 (c) は max_tokens 8192 + 規則 4a + 入力整形 NFKC で 24/24)・参照表示 48→26(残り B 由来)・25→1(#)・rev 1 巡(req 0507 + 追補 1〜3)待ち → EC2 手順書 改訂 1(docs/手順_EC2反映_表示改善第3段_coord_20261009.md)。Dify sandbox で unicodedata は import 可(実機確認 10/09)。

## 表示改善 第 3 段(決215・2026-10-09 反映済み)
- EC2 現行: kg_api/console/shirei タグ s3-20261009(退避 pre-s3-20261009 は 10/16 まで)・プラグイン 0.3.15(識別子 7bfc0821…)・DSL は fed が 0.3.15 識別子で作り直した版(7cd912db / c3c72962)。写し = knowledge_kb_v8/data/api/ec2_s3_20261009/。
- A1〜A6 全部合格(欄なしの既存応答は byte 同一・番号固定・参照表示「(本文は入力の文)」5→0)。
- **所見(未解決)**: 状態整理 LLM が観測文の接頭辞「【観察/試験】」を落とす(訂正の手番 いいえ 0/2 保持・はい 4/5)。kg_api の鍵解決は完全一致 →「一覧に無い質問」で訂正が効かない。番号表には接頭辞あり(LLM の写しの揺れ)。dkb(鍵解決の許容案)・fed(プロンプト側・S6 の判定単位)へ計画の req を出した。実装は承認後。
- Dify の実行記録の読み方: `advanced-chat/workflow-runs` の一覧は作成順でない。`chat-conversations?sort_by=-created_at` → `chat-messages?conversation_id=` → `workflow_run_id` → `node-executions` で辿る(process_data.prompts に LLM の入力全文)。

## 決216 追補 1〜決219(2026-10-09 午後)
- 接頭辞の揺れ = fed の x003 で「NFKC → 先頭の【…】を外す → 番号表と一意一致なら原文へ」(決216 追補 1)。dkb の口は完全一致のまま。
- E1(番号表が手番 2 で {} に戻る・LLM が event を手番ごとに違う文で写す)= 案 A「リセットの印でだけ表を空に」(決217・決209 読み替え)。E2(本番 app.py の 422 は {"error": {code}} でプラグインが detail しか読めなかった)= 0.3.16。
- 決218: 木の「受け取った回答」も R-a(リセットの印)・R-b(訂正を対象ごとに)で fed が dkg_tree.py c4db3b6c に。D-a(状態整理の発話の重複入力を 1 回に)は R の後。
- EC2 現行(Dify 側・決219): プラグイン 0.3.16(識別子 e6f2281b…)・DSL 28c18bf0 / 4baadd52。kg_api は s3-20261009 のまま(R は未反映)。smoke で番号表が手番 3 まで続いた。
- Dify の state LLM の入力には発話が 2 回入る(prompt_template の user 行 + 末尾の query)= EC2 でも確定。

- 決220(2026-10-09 18:3x): 決218 R(dkg_tree.py b25be2c0)を EC2 kg_api に反映(タグ r218-20261009・退避 pre-r218-20261009)。受入 = openapi 同一・diagnose 33/33・facets 差 0・木 22 の全欄(received・q_numbers・mermaid 含む)同一・soak 1 巡正常。
- 案 A keep-alive の観測(2026-10-10 00:2x・22.7 h): kg_api 切断 0・DB debug.log 切断なし → 本採用継続(info 20261010-002x)。
- 決226/227(2026-10-10 01:5x): D-a(状態整理の prompt_template の user の行を外し発話を 1 回に)+ (b)(1 手番目・4 つの保存値が全部空・複数行・event が 1 行目と同じときだけ全文に戻す)を EC2 の D/G に反映(DSL 701792b0 / c648b8af・識別子 0.3.16)。rev R226-1(前の状態の判定が event だけ)→ 是正済み。効き目: 発話の出現 2 → 1・2 回つなぎ 0。turn 1 の event は問いの節を除いた事象の文(規則どおり)。
- EC2 現行(2026-10-10): kg_api 第 3 段 + R(b25be2c0)・console/shirei mermaid-20261009・プラグイン 0.3.16・DSL 701792b0 / c648b8af・nginx resolver 化(決223)。
