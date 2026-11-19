---
name: test-child-must-block-llm
description: ingest_one の関数を試験で回すときは、LLM の口(BedrockClaudeProvider._get_client)と Stage1 を子のプロセスで必ず止める・_commit_extract は補完辞書を書いた後に Stage1 の LLM まで進む(2026-11-09)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-08T17:43:54.174Z
---

2026-11-09、決352 段 2 の 3 で `_commit_extract` の補完辞書の書き方を試験したとき、最初の版の試験は子のプロセスで LLM の口を止めていなかった。そのため、補完辞書を書いた後の Stage1 の LLM の呼び出しまで進んだ(3 回)。手元の AWS の資格は 2 日前に切れていて、認証で断られたので、モデルの呼び出しと費用は無かった見込みだった。それでも「段 2 は LLM 0」の約束を、運に頼って守った形になった。

**Why:** 工程の関数(`_commit_extract`・`commit`・`_finish_candidate`)は、確かめたい書き込みの後に LLM の段へ続くことが多い。試験の材料が合成でも、資格が生きていれば本物の Bedrock を呼び、本文を送る。

**How to apply:** `ingest_one` や工程の部品を試験で回すときは、子のプロセスの頭で `llm_providers.BedrockClaudeProvider._get_client` を例外にする差し替えを必ず入れる。確かめたい所の後の段(`_call(stage1.main...)` など)も止める。試験を回す前に、関数がどこまで進むかをコードで読む。関連: [[verify-in-separate-process]]・[[test-must-not-inject-what-prod-lacks]]
