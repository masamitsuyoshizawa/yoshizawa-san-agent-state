---
name: boto3-default-retries
description: boto3 の client を Config(read_timeout) だけで作ると SDK の再試行が既定(legacy・最大 5 要求)で残る・明示の retries は環境変数より先に効く(2026-11-01 実測)
metadata:
  node_type: memory
  type: reference
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-31T18:55:42.841Z
---

2026-11-01、bkb が手元の偽の口(127.0.0.1 が 429 を返す・資格は偽・boto3/botocore 1.42.91)で、1 回の `converse` に届く要求の数を数えた(coord req 20261101-0350・rep 0354)。

- `Config(read_timeout=…)` だけ = 環境変数なしで **5**(legacy)。`client.meta.config.retries` は `{"mode": "legacy"}` で、試行の数の欄が無い。
- 同じ作り方で `AWS_MAX_ATTEMPTS=7 AWS_RETRY_MODE=standard` = **7**(環境変数が効く)。
- `Config(retries={"total_max_attempts": 1, "mode": "standard"})` = 環境変数の有無に依らず **1**(明示の Config が環境変数より先に効く)。

**How to apply:** 費用を予約して数える呼び出しは、client を明示の `total_max_attempts` で作り、作った後に `meta.config.retries` で照らす(W1 の工程の `llm_usage.install` はそうしている)。共有の `kg_api/kb/scripts/llm_providers.py` の `BedrockClaudeProvider` は read_timeout だけなので、直に使う道(tkb・kg_api の合成など)では 1 回の呼び出しが最大 5 要求になりうる。再送のどれが請求されるかは確かめていない。同じ型の教訓 = [[platform-setting-changes-failure-definition]]。


(索引から移した説明・2026-11-07)
[boto3 の既定の再試行](boto3-default-retries.md)(read_timeout だけだと最大 5 要求・明示の retries が環境変数より先)
