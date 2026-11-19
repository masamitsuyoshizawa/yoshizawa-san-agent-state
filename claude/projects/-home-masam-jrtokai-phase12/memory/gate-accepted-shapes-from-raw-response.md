---
name: gate-accepted-shapes-from-raw-response
description: 門が受け入れる応答の形は API 文書や SDK の解析後の値でなく実サービスの生の応答で決める(Bedrock の記録の設定なし = 鍵 + null を SDK も CLI も隠していた)
metadata:
  type: feedback
---

門(fail closed の判定)が「正常」と認める応答の形は、実サービスの生の応答を見て決める。API の文書・SDK の解析の後の値・CLI の表示から推して決めない。

**Why:** 2026-11-03、暗黙知の U9 の記録の門で、rev は「loggingConfig が null は設定なしと区別すべき」と指摘し、tkb は null を形の不明として止める是正(8131ce5af)を入れた。coord が EC2 で `--debug` の応答本文を見たら、大阪の本物の「設定なし」は `{"loggingConfig":null}`(鍵 + null)だった。SDK(botocore 1.42.91)も CLI(出力長 0)も null を「欄なし」に変えて見せていた。是正の版は本物の正常応答を必ず止める形になっていた(決342 追補 31)。

**How to apply:** 外部 API の応答で分岐する門を設計・点検するときは、読むだけの呼び出しで生の本文を 1 回取り(資格・記録先の名前は出さない・短い本文だけ表示)、許す形をその実物に限る。合成の試験の「正常の対照」も実物の形にする。関連 [[gate-allowlist-not-denylist]]・[[spec-unverified-until-implemented]]・[[bedrock-invocation-logging-sandbox]]。
