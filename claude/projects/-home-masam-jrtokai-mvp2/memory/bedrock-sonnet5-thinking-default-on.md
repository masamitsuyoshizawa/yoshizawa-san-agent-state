---
name: bedrock-sonnet5-thinking-default-on
description: Bedrock の Sonnet 5 は Converse の既定で拡張思考が有効。抽出・転記の呼び出しでは thinking disabled を送る。dgr が写し落として段 B の 5/15 回が文の空のまま上限で止まった(2026-10-30)
metadata:
  node_type: memory
  type: project
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-10-30T06:39:46.607Z
---

Bedrock の `global.anthropic.claude-sonnet-5` は、Converse の既定で拡張思考が有効になっている。抽出・転記の呼び出しでは `additionalModelRequestFields = {"thinking": {"type": "disabled"}}` を送る(phase12 の `llm_providers.py` の登録は `disable_thinking=True`)。

**Why:** 2026-10-30、dgr の `scripts/drawing/vision_call.py` は phase12 から温度の扱いだけを写し、思考の無効化を写し落とした。段 B の 15 回のうち 5 回が出力の上限で止まり、文が空だった。費用も 15 回で予算の 9 割を使った。直したのは a0360f3。dcr の `scripts/dcr/llm_vision.py` にも同じ穴があった(coord 経由で知らせた)。dcr の 21 回(決4・決7)は上限で止まった回 0・空 0 で、出力の短い転記なら思考が付いても上限に余裕があり、穴が表に出なかった(推定)。dcr は 081de02 で直した。

**How to apply:** LLM の呼び出しの部品を写すときは、モデルの登録の全部の欄(温度・思考・出力の上限)を照らす。応答のブロックの種類(text / reasoningContent)を回ごとに記録すると、上限で止まった原因を確定できる。関連: [[grep-hardcoding-before-running-copied-tools]]
