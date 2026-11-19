---
name: bedrock-invocation-logging-sandbox
description: sat-sandbox の Bedrock は東京で呼び出しの記録(送った文と応答の本文を CloudWatch と S3 へ)が有効・大阪は設定なし(2026-11-02 確認)・暗黙知の実送信の扱いは利用者が会社に確認中
metadata:
  type: project
---

2026-11-02 21:2x に coord が EC2(sat-sandbox)のロールで `get-model-invocation-logging-configuration` を読んだ: **東京(ap-northeast-1)= 有効**(CloudWatch・S3 とも・文/画像/埋め込みの配信が真)・**大阪(ap-northeast-3)= 設定なし**。決260 追補 2(10-16)と同じ。記録の設定は呼び出し元の地域ごと。AWS の公式ブログ「Getting started with cross-region inference in Amazon Bedrock」(2024-08-27)に「The logs and invocations will still be in the region and account where the request originates from.」(CloudWatch と CloudTrail・転送先には残らない)とあり、coord が 11-03 に原文で確認した。よって jp の推論プロファイルを大阪から呼べば本文の記録は残らない見込み(jp Opus 5.5 を大阪から呼べるか・権限は未実測)。

**Why:** 暗黙知の U9(実送信)で送るマスク後の文には辞書に無い第三者の氏名が残りうる。東京から送ると AWS の記録に残る。B の工程の NER(決260 の例外で原文)も東京なら同じく残っている。

**How to apply:** 利用者は扱いを会社の方針に確認中で保留(決342 追補 27)。U9 の前に答えを得る。tkb は送る直前に呼び出し元の地域の設定を読み、文の配信が有効なら利用者の dec の許可ファイルが無い限り止める門を実装した(35af8323c・rev 要是正 U9-G1/G2 = 値が null の設定を「なし」と通す・片方の usage の欠けで上界の超過を見逃す → 是正の req 11-03 0130・決342 追補 29)。手元の資格が切れていても EC2 のロール(KbDemoBedrockRole・AmazonBedrockFullAccess)で読める。記録先の名前は書かない。関連 [[aws-account-migration-20261031]]・[[tkb-round0-design-state]]・[[v7-demo-ui-bypass-and-ner-raw]]。

生の応答本文(2026-11-03・EC2 のロール・`--debug`): 大阪(設定なし)= `{"loggingConfig":null}`(CLI の出力は長さ 0)・東京(設定あり)= `{"loggingConfig":{` で始まる object。SDK も CLI も null を「欄なし」に変える。U9 の門はこの実物の形で組み直し中(決342 追補 31)。


(索引から移した説明・2026-11-07)
- [Bedrock の呼び出しの記録(sat-sandbox)](bedrock-invocation-logging-sandbox.md) — 東京は本文を CloudWatch と S3 へ記録・大阪は無し(11-02)・暗黙知の実送信の扱いは会社に確認中で保留
