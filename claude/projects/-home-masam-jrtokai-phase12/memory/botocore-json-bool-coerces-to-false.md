---
name: botocore-json-bool-coerces-to-false
description: botocore(1.42.91)の rest-json 解析は真偽の欄の 'true' 以外(数 1・"x"・配列)を偽に、null の欄と JSON でない本文を「欄なし」に変え、知らない欄を落とす — 解析の結果だけで形の門を作れない
metadata:
  type: reference
---

botocore 1.42.91 の rest-json の解析(bedrock の GetModelInvocationLoggingConfiguration で確かめた・通信なし・2026-11-03):
- 真偽の欄: JSON の true と文字列 "true"/"True" → 真。数 1・0・"x"・"false"・""・[]・{} → 偽。null → 欄が消える。
- 値が null の欄は消える。JSON でない本文・空の本文・空白だけ・`{}` は全部 `{}` になる。最上位が null や配列、object の欄に文字列や数 → SDK の中で例外。
- 出力の形に無い欄は解析の結果から落ちる。

**Why:** 暗黙知の U9 の門(tkb)で、解析の結果の型を照らすだけでは「設定なし」と「空の object」と「空の本文」も、「真偽でない値」も見分けられなかった(8131ce5af は数 1 を文の配信が偽として通していた)。

**How to apply:** 応答の形で安全を判断する門は、解析の前の口(`before-parse.<service>.<Operation>` の事象・`customized_response_dict` で結果を上書き)で生の本文を見て、その回の印を結果に残し、受ける側は印を必須にする。sha を取るなら生の本文から取ることも考える。関連 [[tkb-round0-design-state]]・[[gate-allowlist-not-denylist]]。


(索引から移した説明・2026-11-07)
[botocore は真偽の欄の true 以外を偽に](botocore-json-bool-coerces-to-false.md)(null の欄・JSON でない本文は欄なし・形の門は解析の前の口で)
