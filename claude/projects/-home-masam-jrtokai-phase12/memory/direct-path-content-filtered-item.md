---
name: direct-path-content-filtered-item
description: 別件(決124・2026-09-28): いまの本番の Opus 5 でも、短い問いをそのまま LLM に送る口(直接の経路)で Bedrock の content_filtered が 44%(22/50)出る。fed・ckb が所管の口を洗い出し、発生率と影響(顧客に空回答が届くか)を測って対策案を計画に。実装は別承認
metadata:
  type: project
---

fed が Opus 5.5 化の段 5 前の測定(短い直接の故障の問い 50 問)で発見。直接の経路では Opus 5 が content_filtered 22/50(44%)・Opus 5.5 は 0/50。本番の経路(統合回答の生成・system と根拠つき)では 5.5 は 0/34。つまり問題は「短い問いをそのまま送る形」にあり、モデルの選択とは別。

**Why:** 顧客の短い質問がそのまま LLM に届く口があれば、本番で空回答が高率で出ている可能性がある。

**How to apply:** fed(federate・lite・直接の経路)と ckb(C の口)が所管の口で「短い問いをそのまま LLM に送る経路」を洗い出し、発生率と影響を少数の呼び出しで測り(費用を先に見積る)、対策案(プロンプトの形・system の有無・モデル・再試行)を計画に。dkb・bkb は自分の口に LLM 直送が無いことを確かめる。実装は別承認。[[opus55-production-switch-item]]

**洗い出し(2026-09-28 08:52)**: bkb = accident_router の 5 経路に LLM 直送なし(facet_backend は fed 所有で未確認 → fed へ)。dkb = D-KB の API・プラグインは LLM を呼ばない(確定)。**Dify の D-KB 対話アプリ 2 本(dkg_diagnose_chatflow 3002・dkg_guided_chatflow 4002「対話状態の整理」・fed 所有)は sys.query と会話の記憶を LLM(claude 5 系)へ直送** → fed の対象に。

**ckb(2026-09-28 08:54)**: C の口に直送なし(確定・LLM 呼び出し 7 か所全数)。**所見: llm_providers(kg_api/kb/scripts・9e54be69)は content_filtered で例外を出さず空文字を返し、C の /ask は空回答を「情報が見つかりませんでした」に置き換える → 根拠つき経路でフィルタが出ると顧客に「KB に無い」という誤った答えが届く(推定・未発火・meta.llm の stop_reason には残る)**。C で測るかは fed の Opus 5 本番経路 50 問の結果の後(約 3.7 USD)。対策案候補 = 空の理由が content_filtered ならそうと分かる形で返す(応答の契約の変更 → 案の段で止める)。fed の facet_backend は LLM なし(確定)。

**決126(2026-09-28)**: 直送の口 3 つ(Dify 状態整理 3002/4002 Sonnet 5・自動振り分け質問分類 4002 Sonnet 5・federate c_nl Opus 5 既定オフ)。状態整理が空を返すと code 節点が現象を空にし診断 API は候補 0 件 → 質問が黙って消える。承認 = 3 口の率測定(約 1.5 USD・最悪 3.5・Dify を通さず Bedrock 直接)+ 案 A(空なら sys.query 原文に戻す・決定論・DSL code 節点)の計画化(実装は別承認)。EC2 Dify 5 app は正本と節点一致・差はプラグイン識別子 0.3.9 対 0.3.8 のみ(export は draft・published は未確認)。

**決127(2026-09-28 10:39)**: 測定結果 = Dify 状態整理の写し 100 回・質問分類 50 回(Sonnet 5)・c_nl 34 回(Opus 5)で content_filtered 0。c_nl 残り 33 問は測らない(既定オフ・費用 5.9 USD)。fed の見積り誤り 7 倍(c_nl は 1 問 2 回呼び・schema 同梱で入力 17,578)。案 A の計画 5f229375(fed は A1→A2: 会話変数 3 つで 2 手目以降を守る + B 決定論 1 行)は rev 確認中 → その後 利用者に A1/A2・B・文言・実装を諮る。

**rev 1 回目(2026-09-28 10:53・rep 1053)**: 要是正。A2+B 支持。R1 LLM 例外は code に届かない別経路・R2 code 単体は保存/配線を証明しない・R3 初回と継続で文言を分ける・R4 保存なし≠1 手目・R5 公開版の特定と退避。fed 改訂 1 → rev 再確認 → 利用者へ A1/A2・B・文言・実装を諮る。

**rev 2 回目(11:14・rep 1114)**: 改訂 1(7545e0dd)は要是正・残件 4。C1 空白 query で分岐が排他でない・C2 reenter だけ保存しない仕組みが DSL に無い・C3 現行プラグインは event 空で API を呼ばず「発生現象が空です。」で止まる(計画の「候補 0 件」前提は誤り・rev も 1 回目は見落とし・coord 現物確認 405721a5)・C4 公開版の特定と import 可能な退避が未選択。fed 改訂 2 → rev 3 回目。

**改訂 2(11:21・6a188c2fc315c132)**: C1 分岐 0(空白)を先頭に 5 分岐を順判定・C2 if-else で blank_query/reenter は assigner もツールも通らない回答 2 へ(W4/W5・変異 3)・C3 前提訂正(「発生現象が空です。」で止まる)+スタブ試験・C4 DB で公開版を特定し draft 一致時だけ export 退避。rev 3 回目(req 1121)待ち。

**rev 3 回目 合格(11:30)→ 決128(11:38)**: A2+B・4 文言そのまま・§7 分担 coord 3〜5/fed 1〜2・§7 の後に実装(反映は別承認)。coord 分の結果(info 1141): dialogue_count 1 手目=1・既存会話は _sync_missing_conversation_variables で既定値の変数が作られる・2 app とも公開版(apps.workflow_id)= draft(md5 一致)・EC2 有効 0.3.9 の dkg_diagnose.py 405721a5 = ローカル。残り = fed の 7-1/7-2 → 実装 → 反映を諮る。

**§7 揃い(11:44・info 1144)→ fed は実装(改訂 3・ローカルまで)へ**。7-1 の所見 = refusal_fallback は決129 で別件化 [[dify-refusal-fallback-item]]。7-2 finish_reason は LLM 節点の出力に在る(graphon 0.6.0)→ 改訂 3 で code の入力に足す。

**完了(2026-09-28 12:07・決130〜132)**: 案 A を EC2 Dify の 2 app へ反映(import completed・publish・公開版 D b740cff9 / G 8f9c9ea3 = 正本 D 40bbd917 / G 129853c6・LLM 節点 data 不変・退避 D 670c08eb / G 95191f0d・旧公開版 D df280497 / G 1a729062)。fed の実機確認: 1・2 手目 合格(保存と読出し実証)・空白入力は LLM 節点で Bedrock ValidationException → 400(分岐 0 は Dify では届かない・既存挙動)。決132 で合格・閉じ。改訂 4 は記録のみ。所見: D 2 手目で LLM が現象を書き換え(19→24 字)。取り組みは閉じた。
