---
name: tree-redesign-candidates-history
description: 決266〜287 探索木の図の表示改善 = V2(候補の列挙・経緯)→ V3(3 段 1 項目 1 行・札・左寄せ・【】見出し・L3 の表なし・候補が外れる枝の注記・B 由来の原因名・見えない辺)・EC2 t287(dkg_tree a4ef493d・2026-10-21)
metadata:
  type: project
---

利用者の目視(例題 1 手番 1・Dify D・2026-10-19)の後の修正希望。要望の写し docs/要望_探索木の表示_候補の列挙と経緯_利用者_20261019.md(c7b6f73dc7c31562)。

- 目視の 2 点は仕様: Q2 の「(本文は一覧)」= b_origin(B 由来の質問文は図に出さない・決179)・描く順 = 層(A0/A1 → 未確認)→ 利得の降順(Q 番号は一覧の固定番号)。EC2 の口 /v1/dkg/tree で確認(render.fallback_labels に理由が出る)。
- dkb 設計 docs/設計_探索木_候補列挙と経緯_dkb_20261019.md e1427b1c(改訂 0): 要求の任意欄 views(無ければ byte 同一)・levels(根と各枝で同じ形・B 由来の L1 原因名は出さない・L2/L3 正準名は b_only で出す = coord 判断 追補 1)・history(状態の新しい鍵)・1.75 人日。
- 次: fed の描画側設計(req 20261019-1512)→ 鍵の名を揃える → rev → 実装の dec(利用者)。

**Why:** 点数変化の箱は顧客に読めない。候補名の列挙は決179(B 原文を図に出さない)との線引きが要る。
**How to apply:** 図の表示規則を変えるときは決179 の範囲(原文由来の文)と正準名(統制語彙)を分けて判断する。関連 [[t0-tree-stage1-ec2-deployed]] [[dkg-tree-p2-plan-fed]]

**2026-10-19 18:1x 決270**: 実装着手を承認(fed 改訂 9 8ef1e434・dkb 改訂 6 3dd2e854・rev 条件付き合格 1803)。決267 1 箱・上限 18/20/240・T0 外・直近 4 件 / 決268 → 決269 で (iv) 根を経緯の枠に入れ 3 列・箱 L1+L2・L3 は表・件数 5 種廃止(269 追補 1)・到達性の例外は条件付き(266 追補 13)。EC2 実測 3 回(一時アプリ・削除済み・写し redesign266_mermaid_20261019/ec2_dify*/)。受入 bkb・画面受入は実装後・旗 ON/反映前(Dify 上限 3 USD)・EC2 反映と本番旗 ON は別承認。工数 7.25 人日(推定)。

**2026-10-19 18:49 dkb 分 実装済み・bkb 受入待ち**: ブランチ dkb-tree270 5f41b0ef(main 未投入・受入後に dkb が入れる)。dkg_backend b5ff9d83(集約の鍵を _level2_key/_level3_key/_b_cause_level3_key に切り出し・level_b_only・_state に b_causes)・dkg_tree_state dabc2675(tree_state(..., views, q_numbers_json)・計算後に _apply_views)・試験 test_dkg_tree_views 32 件・変異 10 種全部落ちる。views 無し E2 1,636/HTTP 1,345/木 74 差 0・views あり 5 パス除き 74/74 同一。写し data/eval/tree270_20261019。fed 固定データ fixtures/tree_state_views_fixture.json aabefe89(fed 試験合格)。設計外の細部: levels_reason 鍵は常に置く・not_computed・history は未 enrich で null。fed の test_dkg_tree_warmup は 10/09 から落ちている(偽物に parse_q_numbers 無し・本件無関係・fed へ通知)。実装を別ブランチに置いたのは、dkb を ff すると受入前の実装が main に入るため。

**2026-10-19 19:11 bkb 受入合格(rep 1910)・main 55870c0c へ取り込み**。設計 改訂 7 3310afc6(根が計算されないとき history = null を明記)。残り = fed の旗 ON 実データ比べ・画面の受入・EC2 反映と本番の旗 ON(別承認・coord)。

**2026-10-19 20:24 fed 分も完了・main 966d1739**: fed-tree270 → bkb 受入(1923)→ 240 字は前置き込み(追補 7・8・1930)→ main 9a17ac8b → 画面の受入 手元(rep 1958・7 case × 3 面・不合格 0・変異 3/3・字 +8〜16%・縦 約 2.2 倍)→ F1 根の箱の代替の文が枝向け(「この答えの…で現状とは別」)/ F2 印「新」が名に紛れる →「(新)」(追補 12)→ a3dc38df bkb 確認(2019)→ main e5f58e57・dkg_tree ce2cb5a1a9cc5459・設計 改訂 10 752f703f・撮り直し retake_f12(不合格 0)。道具 = knowledge_kb_v8/scripts/fed/tree270_compare.py(比べ・台の健康)・screen_r270_cases.py / screen_r270_screens.py(3 面の実測)。残り = 決271 EC2 反映・人の判読(coord と利用者・EC2 の写し)・D/G 実経路(3 USD)・本番の旗 ON(別承認)。
教訓: 新しい作業ツリーは git 外データの記号リンクが無く意味照合が黙って落ちた([[worktree-data-symlink-gap]])・240 字の変異の例は旧実装でも収まる長さだと検出できない(前提の検査を足す)・代替の文は根と枝で意味が違う(機械の受入は文の中身を見ていなかった)。

**2026-10-20 決273〜275 第 2 次(V3)**: 利用者の要望 5 点 → 試作 proto273_forms.py で実測(横 3 箱 F2 は Dify の字 3.76 px = 今の 41%・wrappingWidth は手元で効くが Dify で描画失敗(EC2 確定)・辺の札は列の間を約 80 px 広げ字 −7%、2 行で −4.5%)→ 決274 F1-2 採用 → 実装 render_v3(旗 DKG_TREE_RENDER_V3・V2 の上で切替・render_v2 不変)fed-tree273 e9a0d67b → bkb 受入 合格(1152)→ main 383e12cd・dkg_tree 0b2f789e521be121・設計 改訂 12 009f335d → 画面の受入 手元(rep 1200・字 V2 の 95.2〜96.1%・縦 約 1.3 倍・u 上 15/15)。残り = 人の判読・EC2 実測(ec2_candidates_r275.json)・決276 EC2 置換と旗 V3 ON。
教訓: Mermaid は外から枠へ辺があると枠の中の direction が逆に効く(LR が縦)= 頼らない。変異が検出されないとき、試験が実装の関数で期待を作っていた(_visible)・二重の守りで効いていない検査があった(崩れた改行の検査)→ 期待は独立に数え、効かない検査は消して効く守りを緩める変異に替える。

**2026-10-21 決282/283 設計(dkb 改訂 8 3035aa2b → 改訂 9 e3aa4caa)**: candidate_removed の枝にも levels(説明の無い点数変化があれば null のまま)・外れた候補は levels.dropped(levels の中 = 比べの除外 5 パスを増やさない・root_rank 常に整数 + in_top)・決283 で B 由来の L1/dropped の名も本文の cause と同じ文字列で埋める(決179 を L1 に限り緩和・L2/L3 と質問文は不変)。実データでは外れた候補 11/11 が b_sim だった。実装は決284(承認 10:55)。

**2026-10-21 11:09 決284 dkb 分 実装済み・bkb 受入待ち**: ブランチ dkb-tree284 e0e5d085(dkg_tree_state 474a50fe・試験 40・変異 6 全部落ちる・固定データ b35878b4 は受入後に main)。実データ 74: 変更分を戻すと決270 の写しと 74/74 一致・名は本文の cause と 74/74 一致・外れる枝 11 全部に levels。写し data/eval/tree284_20261021。

**2026-10-21 11:33 決284 bkb 受入合格・main 9e871610 へ取り込み**。残り = fed の実データ確かめ・EC2 反映(別承認)。B 由来の key に事故 ID の形(bsim:acc:…)= 識別子なので図に出さない(fed の受入で照らす)・内部状態は key を変えない。

**2026-10-21 決277〜287**: 決277(見出し【L1原因候補】等・左寄せ classDef・幅は広げない = wrappingWidth/frontmatter は Dify で描けない・classDef の width は枠だけ・停止の箱の文)→ 決279(V3 で L3 の表を外す)→ 決282/283/284(候補が外れる枝 = levels.dropped で注記つきの候補の箱・B 由来の L1 原因名 +「(B実例)」・対応表も同じ名・V2 は fed が公開の応答を戻して byte 同一・dkb 改訂 8/9 の鍵)→ 決286(停止の箱を見えない辺 qN ~~~ s で一番下へ)+ R284-2/3(比べの道具の許しを狭める・V3 の meta.masked)→ 決287 EC2 t287。fed の手番は利用者の目視待ち。
教訓: 注記の「上位 n 件の外」の n は dkb の in_top と同じ levels.top を使う(描いた根の数でなく)・fed だけ先に入れると B の名が「候補(名は表示できない)」になる = dkb と同時に入れる・変異は正例が全部合格の上で回す([[negative-test-check-reason]])・Mermaid は列の中しか宣言順に従わない(列をまたぐ一番下は見えない辺)。

## Dify の Mermaid の字は列の数で決まる(旧 mermaid-dify-text-size-by-columns を統合・2026-10-19)

Dify(EC2・mermaid 11.16.0 strict)は svg を回答の枠 576 px に縮めて出すので、flowchart LR の字の大きさは図の横幅 = 列の数で決まる。箱の字数は縦にだけ効く(htmlLabels は節点幅 260 px で自動で折り返す・約 12 字で 1 行)。例題 1 の実測: 今の図 3 列 = 横 1,096 px・EC2 節点幅 130 px / 経緯を右へ直列 = 2,780 px で字は約 4 割 / 経緯を subgraph TB で 4 列 = 1,320 px・105 px / 根を経緯の枠に入れて 3 列 = 1,015 px・換算 139 px。EC2 換算 = 手元の節点幅 260 × 576 ÷ 手元の横幅 × 0.94(coord の EC2 実測で較正)。

**Why:** 「崩れずに描ける」(節点数・字数の上限)だけを条件にすると、読めない図が合格する。

**How to apply:** 図の形を変えるときは、列の数と横幅を先に数え、手元(mermaid 11.16.0 の npm pack・完全版 chromium・strict)で横幅を測って EC2 の字の大きさを換算する。測った図の画像を必ず目で見る(辺を落とした組み立ての誤りで 635 px と出たことがある)。置き場 knowledge_kb_v8/data/dkg_tree/redesign266_mermaid_20261019/。関連 [[screen-acceptance-check-svg-size]]

**2026-10-19 21:xx 決271/272 EC2 反映 完了**: kg_api t270on-20261019(旗 DKG_TREE_RENDER_V2=1・差 10 + profiles 5 + 依存 3 = dkg_c_port/dkg_adjacency/dkg_profile)・shirei t270-20261019(図の下に対応表 + L3 表 + 凡例)・プラグイン 0.3.16/DSL は不変。受入 33/30/22・history/levels 22/22・Dify D/G 3 手番 × 2 合格。退避 pre-t270-20261019・停止コンテナ jrtokai-v9-kg-api-pre-t270on-20261019。つまずき = import 依存の照合漏れ 2 回(D が約 6 分落ちた)。決263-B の EC2 は kg_api/profiles 分だけ完了・プラグイン 0.3.17/DSL は決251 と一緒に。残り = 利用者の目視。

**2026-10-20 決273〜275(第 2 次修正)**: 利用者の目視後の 5 点(辺の札「現状で判断」・q→u 点線削除・箱の中を L1/L2/L3 の段に 1 項目 1 行・停止の箱の文・u 先 s 最後)。実測 = 横に広げるほど Dify の字が小さい(F2 横 3 箱 3.76 px・wrappingWidth は Dify で描画失敗)→ 決274 F1-2(8.67 px)・札 2 行・④ は _SAFETY_LABEL の語・旗 DKG_TREE_RENDER_V3。決275 実装着手(fed 2.75 人日・受入 bkb)。EC2 は別承認(決276)。写し proto273_20261020/ec2_dify/。

**2026-10-20 12:2x 決276 完了(EC2)**: kg_api t275on-20261020(dkg_tree 0b2f789e・V2=1・V3=1)・受入 33/30/22・D/G 3 手番 × 2 合格・退避 pre-t275on(V2 のみ・停止保持)。restart 前に import を確認して無事故。残り = 利用者の目視。

**2026-10-20 18:1x 決277/278(第 3 次修正)完了**: 【L1原因候補】等の見出し・左寄せ(Dify で効く)・停止の箱 3 行文・幅は広げない(枠だけ広がり字が縮む・wrappingWidth/frontmatter は Dify で不可)。EC2 kg_api t277-20261020(dkg_tree ca7b572d・V2=1・V3=1)・受入 33/30/22・D/G 合格・退避 pre-t277。残り = 利用者の目視。

**2026-10-20 20:2x 決279/280 完了**: 図の下の L3 の表を外す(V3・凡例 12 行・対応表は残す)。EC2 kg_api t279-20261020(dkg_tree ffe1d0aa・V2=1・V3=1)・受入 33/30/22・D/G 1 手番 × 2 合格・退避 pre-t279。残り = 利用者の目視(第 3 次 + L3 表なし)。
