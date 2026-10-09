---
name: dify-state-fallback-plan-a
description: 決123〜132(2026-09-28・閉じた): 短い問いを LLM に直送する口の洗い出しと、Dify の D-KB 対話 2 本の状態整理の後ろ盾(案 A・A2+B・EC2 反映済み)・refusal_fallback は現状維持
metadata:
  type: project
---
- 直送の口: Dify の状態整理 3002/4002・自動振り分けの分類 4002(Sonnet 5)・federate の c_nl(Opus 5)。Opus 5 に短い問いを素で送ると content_filtered 22/50、枠(指示・根拠・schema)を付けた口では 0。
- **Dify の空の現象はプラグイン(dkg_diagnose.py 307〜310)が「発生現象が空です。」を返し API を呼ばない**(fed が当初「候補 0 件」と誤った: backend 直呼びの観測と Dify の経路を取り違えた)。
- 案 A(DSL 正本 D 40bbd917・G 129853c6・変換 dify_plan_a.py・試験 32 件): 5 分岐(blank_query/none/last_state/query/reenter)・if-else で保存しない経路・会話の変数 3 つ・LLM の節点は不変。EC2 反映(退避タグは coord の export)・1/2 手目で保存と読み出しを実機確認。**空白だけの入力は LLM の節点で Bedrock が ValidationException → 分岐 0 は届かない**。
- **Dify の Bedrock プラグイン 0.0.79 は Claude 5 系の拒否を、本文前なら global.anthropic.claude-opus-4-8 へ黙って切替(refusal_fallback 既定 true)、本文後なら例外**。切替は plugin の記録も節点実行記録の単価(要求モデルの定義値で一定)からも数えられない。決131 で現状維持。
- LLM の節点は `stream=True`・出力に finish_reason あり(graphon 0.6.0)。`sys.dialogue_count` は 1 手目 = 1。既存の会話は会話の変数を既定の空で作る。

**Why:** 同じ取り組みを再開するとき、「実機で届かない分岐」「数えられない切替」を前提に置く必要がある。
**How to apply:** Dify の LLM 節点の後段に後ろ盾を足す案は、LLM の例外経路(拒否・空白入力)には効かないことを前提に書く。記録の口の有無は「毎回出るはずの記録」があるかで確かめてから 0 件を読む。関連 [[opus55-production-switch-item]] [[estimate-count-calls-and-inputs]]
