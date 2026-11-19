---
name: b-document-vs-masked-files
description: B 正本の Document.text は決86〜95 の地名の戻し済み(正)・accident_kb_v7/data/masked は是正の前の版(戻す道)。本文の版の新旧は更新日でなく工程の記録で決める(2026-10-27 に向きを逆に報告し訂正)
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-26T20:18:16.847Z
---

2026-10-27(決312 の準備中): bkb が「Document.text は 348 事故で今のマスク済みファイルより古いマスクの版(差 1,392 箇所・ファイル側は [氏名])」と info 0503 で警報を出し、coord が決313 で利用者に諮った。実際は**向きが逆**。
- 1,392 箇所は、2026-09-25 に利用者の承認(決86 (4)・88・91・94・95)で、Document.text の [氏名] を地名へ戻した位置(dkb の unmask_fields・ingest_pipeline の積荷 unmask・G16「文書 1392 か所」・記録 accident_kb_v7/data/pipeline_state/unmask__commit__20260925.json と _ec2)。
- `accident_kb_v7/data/masked` は是正の前の版で、決88 諮2 で戻す道として意図して残してある。build_fulltext.py を全件で回すと是正が巻き戻る(守り = unmask_fields --check-rewind・監査 X9b)。
- 決312 追補 2: 事故摘録の類似検索の索引は B 正本の Document.text から作る(/v1/accident/doc と同じ本文)。masked は使わない。
- bkb が件数で見た型(6 種の文字列・全部 2026-07 追加の表の形式の系統・PDF の系統では同じ字がもともとマスクされていない)は地名の戻しと矛盾しない。

**Why:** ファイルの更新日(06-05 と 07-08)だけで新旧を推定し、正本への後からの書き込み(積荷 unmask)と決88〜95 を確かめなかった。記憶([[dkb-coverage-singularity-bkb]] の「マスク 47 件は決86 で地名と判定」)に手がかりが在ったのに照らさず、個人情報の警報として coord → 利用者まで上がった。
**How to apply:** B の本文・マスクの版の新旧や「漏れ」を言う前に、pipeline_state の記録(unmask・normalize 等)と、その欄に触れた dec を確かめる。警報を出す前に、[[assumed-current-state-without-checking]] と [[read-dec-addenda-first]] の型でないかを照らす。
