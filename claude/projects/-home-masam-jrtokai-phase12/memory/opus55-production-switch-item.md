---
name: opus55-production-switch-item
description: 決106〜125: federate の fused 合成を Opus 5.5 global(effort low)へ切替済み(2026-09-28・EC2 反映・切り戻し先 pre-opus55-20260928)
metadata:
  type: project
---

利用者が 2026-09-26 に「federate の評価に Opus 5.5 を使えないか」と問い、fed の調査(rep 20260926-2045)で Opus 5.5 は東京で AUTHORIZED(jp./global.・apac 無し)・CountTokens は不可・公表単価は Opus 5 より約 2 割安(推定)・llm_providers.py に未登録と分かった。評価だけ 5.5 にすると本番(bedrock-opus-5)と別の系を測るので、利用者は「本番の合成も 5.5 に替える案を別件に」を選んだ(決106)。

**2026-09-27 計画 改訂 0 を提出**(docs/計画_federate合成のOpus5.5化_fed_20260927.md・77c9de14・rep 20260927-1712・承認待ち): 替えるのは federate の fused 合成だけ(federation.LLM = KG_LLM_PROVIDER は app.py の他の口と共有なので、federate_backend に FED_FUSE_PROVIDER を足して分ける・EC2 は置換方式で環境変数が変わらないのでコードの既定で切替)。**単価は AWS Pricing API で確定**(ServiceCode `AmazonBedrockFoundationModels`・ap-northeast-1。`AmazonBedrock` には Anthropic の行が無い): 5.5 global 4/20・jp 4.4/22・キャッシュ読み 0.2/0.22。推し jp(国内処理)。評価は S1 50(5.5 × 2 + 決107 の Opus 5 新1/新2 の保存回答の再利用・J1 1600 × 2・約 9 USD 推定)。**「LOO・判定器 v2」は D-KB の物差しで D-KB は LLM を呼ばない**ので読み替えを coord に確認中。kg_api が読む llm_providers は kg_api/kb/scripts/ のもの(OWNERS に無い)。

**2026-09-28 段 4 完了(rep 20260928-0030)**: 利用者は global を選択(決118)。**Opus 5.5 は思考を無効にできない**(`thinking.type.disabled` は ValidationException・adaptive 常時・effort low〜max)→ 決119 で BedrockClaudeProvider に thinking_effort(既定 None)を足し 5.5 は low。段 3 = FED_FUSE_PROVIDER で合成だけ分離(取得用は暗黙の lite を含め従来のまま・2 段 API は確定値を継承・fuse_input に tagmap/cite_set の sha)。S1 × 4 系列(同じコード・入力 sha 全問一致)で ΔCP +0.5・ΔINC −0.5・content_filtered 0・所要中央 16 秒対 31 秒・1 系列 2.6 対 3.7 USD・通算 15.09 USD。**疎通で短い直接の故障の問いが 2 回とも content_filtered(本文が空)** → 段 5 の前に短い問いでの発生率を測ることを推した。段 5(既定の切替・EC2)は別承認。

**2026-09-28 段 5 完了(本番切替・EC2 反映・rep 20260928-1009)**: 短い問いの測定(決123/124)で本番の経路は 5.5・Opus 5 とも content_filtered 0/50、**直接の経路で Opus 5 が 22/50**(5.5 は 0/50)。FUSE_PROVIDER_DEFAULT = bedrock-opus-5-5-global(1753b32b)・EC2 反映(coord・退避タグ pre-opus55-20260928 = 切り戻し先)・EC2 で 3 問 5.5/end_turn・federation.LLM は opus-5 のまま。通算 22.33 USD(目安 25)。**教訓: 切り戻し先は「反映先に実際に在った版」を書く(ローカルの段の版を書いて coord に訂正された)**。残り = 決126(直送の口の測定・案 A の計画)。

**Why:** 順の明示の是正と同時にモデルを替えると効果を分けられない。モデルの変更は答えと費用が変わる別の承認事項。

**How to apply:** fed が計画を書く(登録・地域・単価の照合・LOO と判定器 v2 での答えの変化・EC2 反映)。着手は順の是正の評価(決106 (3))の後。承認は利用者。[[storage-order-dependent-responses]]

**計画 改訂 0(2026-09-27 17:12 fed・77c9de146be43c11)**: fused の合成 LLM だけを替える(lite・判定器・他の口は不変)。単価は AWS Pricing API で確定(5.5 = global 4/20・jp 4.4/22)。段 0 単価表 → 1 llm_providers 加法 2 行 → 2 疎通 → 3 FED_FUSE_PROVIDER → 4 評価(S1 50・5.5 2 系列 + Opus 5 保存回答再利用・J1 1600×2・約 9 USD・目安 20)→ 5 切替。coord: rev へ確認 → 利用者に jp/global・段 4・基準を諮る。llm_providers.py を OWNERS に「共有・窓口 coord・加法のみ」で登録。決106 の「LOO・判定器 v2」は誤記(federate は S1+J1)。

**rev 確認(2026-09-27 17:27)**: 要是正 3 = O55-1 高(provider_id が C 取得(NL→Cypher)と合成で共用 → 合成専用の選択を分離・/federate/dkg と 2 段 API の対象/非対象)・O55-2 中(採る基準を数式で・既承認 TOLERANCE c+p 低下 ≤1/incorrect 増 ≤1 との関係・(c) の閾値)・O55-3 中(段 2 の cachePoint 前提は現行 cache_system=False と不一致)。判定は 100 呼出し(400 判定値)。決114 = fed 改訂 1 → rev 2 回目 → 利用者。教訓: 「合成だけ替える」は差し替え点 1 つでは守れない(取得と合成が同じ引数を共用)。

**改訂 1(2026-09-27 19:05・ca13acf690dde45d)**: fuse_provider_id を足し合成だけが使う(C 取得・A は provider_id のまま・2 段 API は retrieval.fuse_provider・/federate/dkg 対象)・P1 で未設定時の不変・否定例 N1〜N7・基準 ΔCP ≥ −1 かつ ΔINC ≤ +1(TOLERANCE の値のまま・集計単位の読み替えは承認事項)・(c) 未展開 0・acc/doc ≥ 0.999・キャッシュなし・判定 100 呼出し・jp = 東京と大阪。rev 2 回目 req。

**rev 2 回目(2026-09-27 20:48)**: 要是正 2(中)。O55-1 残件 = 暗黙 lite(provider=None)で取得側の Sonnet を保つ記述(計画どおりだと C 取得が Opus に戻る)・N4 に C 取得・N6 逆向き。再利用条件 = user_sha256 は citation を含まない → tagmap/根拠集合の同一性も照合・欠けば旧 2 系列を再生成(約 16 USD)。(c) は「現行実装の閾値 0.999」。O55-2/3 は解消。決116 = fed 改訂 2。

**改訂 2(2026-09-27 21:00・d2cb450fdfcc491c)**: 取得用 provider は現行の解決のまま・合成用だけ分離・N4 に C 取得・N6 両向き・**保存回答の再利用をやめ 4 系列とも再生成(約 16〜17 USD・通算 20 の内)**・fuse_input に tagmap_sha256/cite_set_sha256・(c) 現行 F-2 の 0.999。dkb 改訂 1 とそろえて rev 3 回目。

**rev 3 回目(2026-09-27 21:09)**: 改訂 2 は条件付き = B-1 cite_set_sha256 を「監査(audit_citations)に渡す全出典と裏付け ID(extra.acc_id/acc_ids・_M/_Q)の集合」に定義・否定例。費用 jp 約 17.4 / global 約 16.9 USD(推定・20 目安内)。決117 = fed 改訂 3(小)→ coord が追記を確認(rev 再確認不要)→ 利用者に jp/global・段 4・基準を諮る。

**決118(2026-09-27 21:19・利用者)**: **global**(jp の推しは採らず)・段 0〜4 承認(段 0 単価表 = coord が bedrock-opus-5-5-global 4/20/0.2/5 を追加)・基準 ΔCP ≥ −1 かつ ΔINC ≤ +1(2×2 平均・TOLERANCE の値のまま・集計単位の読み替え)+ 失敗 0・上限到達 0・未展開 0・整合率 ≥ 0.999・決定論指標非悪化・上がっても採る理由にしない。見込み約 16.9 USD・目安 20。段 5(既定切替・EC2)は別承認。fed が段 1〜4 へ。

**段 2 で停止(2026-09-27 21:23)**: Opus 5.5 は適応的な思考が常に有効(thinking.type.disabled 不可・adaptive + output_config.effort)。ValidationException で台帳停止(0.006 USD)。**決119 = 案 A effort low**: BedrockClaudeProvider に effort 引数(既定 None・5.5 の 2 項目だけ low)・台帳解除・疎通 3〜5 回で思考量/所要/max_tokens を測り段 4 の見積り直し → 再承認。教訓: モデルを替える計画は「同じ設定」を前提にせず疎通を最初に。

**疎通と段 3(2026-09-27 22:07/22:24)**: effort 引数 +37 −0(0e545739)・台帳解除 22:04・疎通 global/low = 統合回答 2 問正常(0.066/0.053 USD・58.9/16.8 秒)・**短い直接の故障の問いは 2 回とも content_filtered で本文が空**。段 3 完了(FED_FUSE_PROVIDER・80b19add・前後同一)。**決120 = 段 4 は目安 20 のまま(S1)・content_filtered は基準 (b) の失敗に数える(1 件でも出れば 5.5 は採らない)・発生条件を別に記録。** 短い問いでの発生率は本番採用の条件として別に測る。

**段 4 結果(2026-09-28 00:30 fed)**: 基準 (a)(b)(c) 充足(ΔCP +0.5・ΔINC −0.5・失敗/空/content_filtered 0・acc/doc 1.000・判定ゆらぎ 0・実費 15.09 USD)。5.5 は所要 中央 16 秒(対 31)・1 系列 2.6 USD(対 3.7)。**決123 = 段 5 の前に短い直接の問い 50 問で content_filtered の発生率を Opus 5 と比べる(約 3 USD・判定なし)**。結果で段 5(既定切替・EC2)を諮る。

**短い問いの測定(2026-09-28 01:19・途中)**: 直接経路の content_filtered = Opus 5 22/50(44%)・5.5 0/50。本番経路(統合回答)の 5.5 = 0/34(end_turn)。5.5 の直接経路の空 2 は思考が max_tokens 600 を使い切ったもの。認証切れで停止(通算 18.02)。**決124 = 再ログイン → 解除 → 残り 16 + Opus 5 の本番経路 50 問(約 3.7)・目安 20 → 25**。段 5 は結果で。effort low でも思考が出力上限を食う → 本番の max_tokens 設計に注意。

**再開(2026-09-28 09:02)**: 利用者が再ログイン(sts 確認)→ fed へ再開の合図(info 0902)。残り = 本番経路 16 問(5.5)+ Opus 5 本番経路 50 問 → rep → 段 5 を諮る。

**段 5 実施(決125・2026-09-28)**: 短い問い 50 問の本番経路で 5.5/Opus 5 とも content_filtered 0(直接経路は 22/50 対 0/50)→ 利用者承認 → fed がローカル既定 bedrock-opus-5-5-global(1753b32b)→ coord が EC2 へ 3 本反映(タグ pre-opus55-20260928 / opus55-20260928・反映前 federate_backend は f1a9570930463f62 で fed の言う 699bdfa0 では無かった)→ fed の 3 問確認待ち。通算 22.17 USD(目安 25)。

**完了(2026-09-28 10:1x)**: EC2 で 3 問合格(provider bedrock-opus-5-5-global・federation.LLM は bedrock-opus-5 のまま)・タグ opus55-20260928・通算 22.33 USD。EC2 の Opus 5 対 5.5 の所要は未測定。
