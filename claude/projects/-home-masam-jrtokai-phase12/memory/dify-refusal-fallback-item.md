---
name: dify-refusal-fallback-item
description: Dify の Bedrock プラグイン 0.0.79 は Claude 5 系の拒否(refusal/content_filtered)を本文前なら Claude 4.8 Opus に黙って切り替え、本文後は InvokeError にする。決129(2026-09-28)で別件化
metadata:
  type: project
---

Dify(EC2・CE 1.16.1)の Bedrock プラグイン 0.0.79 は、Claude 5 系(Sonnet 5 global を含む)の stop_reason `refusal`/`content_filtered` を「拒否」として扱う(fed が 2026-09-28 にソースで確定・info 20260928-1142)。
- 本文が出る前の拒否 → `refusal_fallback`(既定 true)で **Claude 4.8 Opus に黙って呼び直す**(利用者にも記録にも出ない)。切り替え先が呼べるかは未確認(kg_api では廃止済み ID の注記)。
- 本文(`<think>` を含む)が出た後の拒否 → InvokeError。DSL の LLM 節点(3002/4002)に error_strategy 無し → ワークフローがエラー終了。
- 案 A(状態整理の空応答の後ろ盾・決128)はこれに効かない(効き目は正常終了の空と不成立だけ)。
- 附記: Dify の claude-5 の effort 既定は high。DSL は指定なし。決126 の測定(写し)は Dify を通していないので切り替えは観測していない。

**決129(2026-09-28)**: 別件として調べる。fed が (1) 切り替え先が呼べるか(直接 1 回)と (2) 対策案(refusal_fallback 無効 + error_strategy / 記録 / 現状維持)を計画に。案 A の実装を先に。実装は別承認。

**Why:** モデルが黙って替わる経路は、出典・決定論の規律(V6 哲学)と食い違い、記録にも残らない。
**How to apply:** Dify 経由の Claude 5 系で「答えの質が急に違う」「例外でエラー終了」を見たら、まずこの切り替えを疑う。関連 [[direct-path-content-filtered-item]] [[dify-demo-ec2]]

**決131(2026-09-28 12:05)= (c) 現状維持・閉じ**: 切り替え先 global.anthropic.claude-opus-4-8 はローカル資格情報で呼べた(EC2 未確認)。(b1) 節点実行記録の usage 単価は全件モデル定義値(0.003/0.015)で一定 → 切り替えは数えられない。見直しは Bedrock プラグイン更新時。
