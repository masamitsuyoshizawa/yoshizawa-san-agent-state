---
name: demo-plans-approved-20260924
description: WEB デモ 2 つ(T0 探索木画像・B-KB 第 1 段効果)と API 完全仕様書の計画 4 本を利用者が承認(決41・2026-09-24)。配置は b-kb-v3./t0.・console で API 全機能(doc は今のまま・admin は載せない)
metadata:
  type: project
---

2026-09-24 決41(dec 20260924-1056 demo-plans-approved): 利用者が計画 4 本を承認。fed 効果デモ+仕様書 8a5c0f02a7c602b6・fed T0 画面 改訂 1 216c39de7d03d15b・fed console 全機能と /v1/t0/tree と配置 改訂 1 44e165399a64027b・dkb 図の生成 第 5 版 214873769bfdebf9。決38 = 配置は別 URL・同じ形(b-kb-v3.54-250-247-97.nip.io・t0. 新設・旧 v3. は移行期間後に coord が期日を決めて外す)・/v1/t0/tree は kg_api に加法・console(所有 fed・OWNERS 済み)で API 全機能。決39 = T0 閲覧者は既存 Basic 認証・印必須(内部試作・専門確認 17 項目未回答・取り外しを含む経路・顧客提示は段階 2 の承認が先)。問1 = console の doc(摘録全文)の口は今のまま(新頁は原文を持たない)・問5 = admin/schema_cache/clear は載せない(唯一の例外)。T0 の木と評価は配置前にローカルで作り kg_api は ro マウントの検査済み JSON を要求ごとに sha256 照合して返すだけ(hash 不一致は 503・他の口に波及しない)。

**Why:** 利用者が「見え方の WEB デモを先に」「第一段の効果が分かる形」「console/v10/shirei と同じ形」「API の最新版・完全版の仕様書」を求めた(2026-09-24)。
**How to apply:** 実装は承認した sha の版が正本。EC2 配置(証明書 2 件・nginx server 加法・kg_api app.py の置換は EC2 現物から差分・console 再ビルド)は coord。仕様書は openapi 6889d0904a7f472e を正とし T0 の口は配置後に採取して追記。原文は画面・API・連絡文に出さない。[[v3-stage2-s3-plan-state]] [[fault-tree-stage3-fed]]

**2026-09-24 11:12 追記**: 決42 = 受a5「原文の語が画面に無い」は利用者決定で「辞書の語以外の欄で要約との一致 0」と読む(現象辞書の表層 23〜25 字が摘録の 20 字窓と重なる 2 語は統制語彙として出す・段 8 の API と同じ)。型 B の例 = デ0 8 番(旧 199 件に埋もれた 8 件)。「1条目のガイシ損傷」は B に該当 0 件(隠さない)。EC2 の B graph_digest 89fea892dd605ddd(bkb-gd-5)を配置前に coord が照合。W2 生成物 kg_api/data/effect_demo/effect_demo_v3v4.json 70b32e666821c994(git 外)。決43 = T0 描画の R4-b で version「 Ver.1.1」の 8 字一致は版の識別子として固定の形に限り許容(計画 第 6 版へ・利用者に報告)。dkb 描画実装 d237f74f(R1〜R7 実データ PASS・R4-b のみ上記)。

**2026-09-24 11:25 追記**: fed W3(効果デモのタブ・958012ab)/W5(/v1/t0/tree・t0_router.py 2730cdc8・app.py 加法 5 行・build_t0_demo.py)/W6(T0 画面 ft0_tree_demo.py 19038683)実装済み・ローカルのみ。coord 別プロセスで試験 15/13/7 PASS。決44 = table の warning は外したまま(HB/M1 と 9〜11 字一致・戻すのは利用者承認)・評価器の版 ft0-modules-digest-v1 は実行が読む 4 本で定める・実データの筋書きは E0/E2/E2(S2 は合成 fixture のみ)。決43 = R4-b の version 許容(dkb 第 6 版 7b687078)。EC2 に置く物: T0 置き場 t0-20260915-2213(manifest sha256 7e824ab3…= KG_T0_MANIFEST_SHA256)・効果デモ JSON 70b32e66。残り W7 console・W4 仕様書・W8 手順書 → coord が EC2 配置(B digest 照合・凍結試験を先に)。

**2026-09-24 11:50 配置完了**: URL = https://b-kb-v3.54-250-247-97.nip.io/(段 8 + 効果デモ)・https://t0.54-250-247-97.nip.io/(T0 画像デモ・印)・https://console.54-250-247-97.nip.io/(全機能)。既存 Basic 認証。旧 v3. は 2026-10-08 に coord が外す。kg_api app.py 58fb9de6 → d9da2789(+5 行)・タグ pre-t0-20260924/t0-20260924=latest・退避 ~/kgbak_20260924_t0。nginx 588→676 行・退避 ~/nginxbak_20260924・タグ pre-demo-20260924/demo-20260924。画面タグ pre-effect/effect・pre-allapi/allapi・t0-20260924。証明書 /certs/streamlit-b-kb-v3・streamlit-t0(期限 2026-12-23)。openapi 配置後 f377e4a9f1e730a4(33 経路)。画面目視は利用者の環境で。

**API 完全仕様書(最終)**: docs/仕様_API_v1_facets-v4_完全版_20260924.md bcd89b8e5cf208a1(670 行・33 経路・配置後 openapi f377e4a9 から生成・試験 9/9)。fed の見どころ: 効果タブ = デ0 1 番(旧 1 候補/新 2 候補)・8 番(埋もれ 8 件)/ T0 = 印・決44 注記・s2 で E2 と図 / console = 新 8 口あり・admin なし。画面目視は利用者待ち。

**API 完全仕様書(S7b 後・最終)**: docs/仕様_API_v1_facets-v4_完全版_20260924.md ac3fdd0bf04c5191(709 行・配置版 942c6c74・meta 18 鍵・§1-8 読み先/vocab_files 4 鍵/503 3 通り/札 17/上限 3・例は s7b_resp 33 件・試験 11/11)。前版 e7ea17ee は S7 前(JSON 経路)。

**2026-09-24 16:40 旧 v3. URL 除去(利用者指示で前倒し)**: nginx の段 8 server 44 行を外し 676→632 行(86894fde)・タグ pre-v3rm/v3rm-20260924・退避 ~/nginxbak_20260924_v3rm。現行 URL = https://b-kb-v3.54-250-247-97.nip.io/(切り口の探索 + 効果デモ)・https://t0.54-250-247-97.nip.io/(T0 画像デモ・印)・https://console.54-250-247-97.nip.io/(API 全機能・admin 除く)・v10./shirei. 不変。Basic 認証は既存。証明書 streamlit-v3 は残置。

**2026-09-24 17:08 画面の不具合と マニュアル**: 利用者報告「Json Parse Error … "v1" is not valid JSON」= 画面が文字列の値を st.json に渡していた(fed 所管)。fed 是正 app_streamlit_facets_v3.py cd7750ef4e353019(show_value: dict/list だけ st.json・他は st.code・試験 18)。EC2 段 8 コンテナ作り直し(タグ pre-fix1/fix1-20260924)。マニュアル HTML docs/マニュアル_B-KB-V3_画面と出力_20260924.html 3324dc41b11ebdd0(coord・Artifact https://claude.ai/artifact/SwcN6ttq5qXGj3QkMRETxH・fed の内容確認待ち)。console 105 行の同型は fed が確認。

**マニュアル 第 2 版**: 189004ef3a88689b(fed の確認 rep 1711 の 8 件 + 7 件を反映・Artifact 第 2 版 同 URL)。console 105 行の同型は fed が「文字列だけを返す口は見つからず(変数の型は未確認)」。

**2026-09-24 17:20 決68(利用者指示)**: T0 デモの対応質問例をできる限り増やす(未確認でもよい)。いまは t0_questions.json に 1 問(q:t0:turnout-no-throw・snapshot 2213・経路 11 + 側枝 3)。条件 = 質問表に確認の段(confirmed/unverified)・質問ごとに「経路は未確認」の印(決39 の印は不変)・枠は不変・(a) snapshot 内の別の起点・枝 → (b) 他の T0 シート抽出(評価器合格は不要・R4 原文なし必須)。割当 dkb 候補列挙+経路+R1〜R7 / fed 複数行対応+全件生成+manifest+再起動なしの配置手順 / coord 配置。順 = dkb 候補一覧 info → 利用者に見せる → 生成 → 配置。
