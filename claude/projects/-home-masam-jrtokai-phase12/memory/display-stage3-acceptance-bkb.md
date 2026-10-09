---
name: display-stage3-acceptance-bkb
description: 表示改善 第 3 段(決207〜212)の bkb 受入の状態・道具と置き場・残りは S6 (c) の LLM 24 件(AWS 認証待ち)
metadata:
  type: project
---

2026-10-09: 受入設計 docs/設計_受入_表示改善第3段_bkb_20261009.md 改訂 0(8ace8478)。最終の版(main a029b577)で S6 (c) 以外合格(rep 20261009-0506)。所見 F1(shirei 開発者モードの curl が木の道で口と本文が不一致)・F2(L1〜L3 の文の改行で Dify の行が割れる 6/200)。

**Why:** 次に回すのは S6 (c)(状態整理の LLM 24 件)だけ。この PC は AWS(saml)が切れていて、利用者の saml2aws login 待ち(coord が依頼済み)。

**How to apply:**
- 道具: knowledge_kb_v8/scripts/bkb/display3_acceptance.py(S1/S5/S7/S8/S9/S10)・qnum_acceptance.py(S2〜S4・S6・S11・`--phase llm --limit 1` で試してから 0)。venv/bin/python3 で本体ツリーから回す。
- 置き場: knowledge_kb_v8/data/eval/bkb/display3/run_20261009-0350_final・qnum/run_20261009-0350_final(CURRENT のファイルにも書いてある)。
- 教訓: 事象の id に | が入る(rsplit で束ねる)・木の一覧の質問の鍵は label_full・shirei の特異点は actions_response・AppTest の st.iframe は UnknownElement の proto.srcdoc・入れ子の走査で図が重複して数わる(異なる図で数える)・スタブの口の番号は伏せる・症状の守りは 3 重(_prep の全角・許可の文字・行の文法)で 1 か所の変異は等価になる・この PC の GPU は 6 GB(同時 2 本でも遅い)。
- Dify の LLM 節点の組み立て(2.0.0-beta2 のソースで確定): system → prompt_template の user → 記憶の履歴 → query_prompt_template。EC2 の 1.16.1 で同じかは未確認。

2026-10-09 05:12: F1・F2 の是正(shirei a78794f3・プラグイン bb67b3dd)を確かめ合格(rep 0512・写し display3/run_20261009-0511_f12)。残りは S6 (c) だけ。

2026-10-09 09:09: S6 (c) を EC2 ホスト(大阪・global.anthropic.claude-sonnet-5 = jp. は無効)で回し 13/24(rep 0909)。訂正の発話で max_tokens 2048 を Sonnet 5 の思考が使い切り本文が空 10 件・写しの 1 字(～→~)1 件。最後まで返した 14 件中 13 件正。教訓: 答えた質問は asked で渡すと次の返信から消える → 今の返信に無い番号の現実の場面は訂正・EC2 ホストの converse は usage を返すので費用を実測できる・Sonnet 5 の思考は出力の上限に数わる。

2026-10-09 11:30: 決213/214(max_tokens 8192・規則 4a・入力整形の NFKC)の後の版で S6 (c) 再走 24/24 合格(rep 1130・手元 AWS_PROFILE=saml)。判定は LLM の出力を x003 に通した後で見る(本番と同じ道)。生の出力では 23/24・1 件は NFKC が救った・出力の最大 2,413 トークン。これで第 3 段の S 項は全部合格・残りは EC2(別承認)。

2026-10-09 11:43: rev rep 1130 の表3-1(代替先の番号表 422)・表3-2(shirei の木の失敗を保持・リセットで消えない)を受け、受入設計 改訂 1(52bf7ac03c2ed5aa)に S12・S13 を足した(rep 1143)。実走は fed の是正の rep の後。上限の値と再試行の条件は fed の rep から実走の前に固定する。是正の前の版で不合格になることも確かめる。S6 (c) の訂正は質問のみで、測定 id の訂正は未実施(coord の判断待ち)。

2026-10-09 11:53: rev 表3-1/3-2 の是正(fed 5e1c9c45)の受入 合格(rep 1153・設計 改訂 2 f6b88e0d56924a12・道具 rev32_acceptance.py/judge_rev32.py・証跡 rev32/run_20261009-1148)。是正の前 ed3ae84c は S12 系列と S13 (a)(b)(c) が落ち、変異 4 つも全部落ちた。測定 id の訂正 LLM 4/4(BKB_LLM_MEAS=1)。答えた測定は次の返信にも残る(質問は消える)。所見(低): shirei の失敗の保持は上限なし。残りは EC2(別承認)。

2026-10-09 13:10 EC2 受入(rep 1310・証跡 ec2_s3/run_20261009-1253): (1) 前後差 0・(2) 番号 66 手番と 422・(3) 画面は合格。**E1**: Dify で手番 1 の状態整理 LLM が event を発話 2 回分で写し、手番 2 で x003 の _q_table(event 完全一致)が表を {} に戻す(今回 2/2・会話記録 3/7)。**E2**: 本番 422 は {"error":…} でプラグインが読めない([[test-app-lacks-prod-error-envelope]])。E3: 改行で特異点が変わる見本 1 件。S6 (c) は手番 1 の event も手番間の event の一致も見ていなかった。

2026-10-09 13:35: 決216 追補 1(x003 接頭辞照合・DSL 641ec991/9573723c)受入 合格(rep 1335・道具 x003_prefix_acceptance.py)。x003 の照合は送る表に頼るので E1(event の揺れで表が {})の是正後に G 手番 3 を再度。決217 Q-3 の準備: prod_error_shape.py(app.py の http_exc を ast で切り出す)で S6 (b)・S12 に本番の形を追加。b9a6 では本番の形 2/10。0.3.16 の受入では 10/10 が線・rev32 の WANT の plugin sha を更新すること。

2026-10-09 13:48: 決217(E1 案 A・E2 0.3.16・fed fb1021e7・DSL f4255202/8e162dd4)の受入 合格(rep 1348・設計 改訂 3 6294eab5・道具 dec217_acceptance.py・証跡 dec217/run_20261009-1345)。EC2 の会話記録を x003 → プラグイン → x010 で再生する型を作った(Dify の with_tree・top は tool_parameters でなく **tool_configurations** にある・最初は読み落として診断の道で回した)。残り = EC2 反映(別承認)後の実機の 3 手番。

2026-10-09 15:03 決219 実機確認 合格(rep 1503・live219/run_20261009-1454・0.7123 USD): 4 手番 × 2 事象 × D/G で表の継続 12/12・以前の Q1 の訂正 4/4・接頭辞 3/3 戻す。422 は公開 API の会話変数の更新(PUT /v1/conversations/{id}/variables/{vid}・Dify 1.16.1 で 200)で実機に起こせた。所見 L1(R 前の受け取った回答)・L2(422 の手番の接頭辞落ちが保存に残る)。
2026-10-09 15:33 決218 R-a/R-b 受入 合格(rep 1533・received_r/run_20261009-1443・281+44 手番・前の版は変わる 7 種で 0・received 以外 325/325 同じ)。EC2 実機の手番を手元の新旧で再生し、前の版 = EC2 の画面の行と一致。残り = R の EC2 反映後に実機で手番 3・4。

2026-10-09 17:39 決218 R 再受入 合格(rep 1739・設計 改訂 4 4db37da0・received_r/run_20261009-1608_r218)。rev R218-1 = 全欄の差を訂正にしていた(注記だけの差で同じ答えを今回分に)→ fed 是正で訂正は DkgObservation の 13 欄の差だけ・定義外だけの差は退避。bkb の初回の受入(1533)は答えの欄の差しか作らず R218-1 を見逃した(境界の場合を足していなかった)。欠損を同一と数えない判定に直した。

2026-10-09 18:47 決220(決218 R の EC2 反映)実機確認 合格(rep 1847・live220/run_20261009-1842・0.5969 USD)。手番 3 = 以前の Q1 の いいえ 1 件 4/4・手番 4 = 追加 1 件 4/4・リセットで行なしと表 {}。L1 解消。event の揺れは手番 2 だけで起きた。表示改善 第 3 段と関連の是正(決209〜220)の bkb 受入はこれで全部済み。

2026-10-10 00:55 決226(D-a = 状態整理の user の行の除去 + (b) = 1 手番目で複数行の発話の 1 行目だけを写した event を全文に戻す)受入 合格(rep 0055・d_a_b/run_20261010-0055・道具 d_a_b_acceptance.py)。DSL 74fa38e3/e173d96b。D-a の効き目(2 回つなぎの減り)は EC2 反映後に会話記録で数える。

