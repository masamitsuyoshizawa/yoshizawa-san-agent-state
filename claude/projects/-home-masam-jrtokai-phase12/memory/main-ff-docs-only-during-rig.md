---
name: main-ff-docs-only-during-rig
description: 利用者の決め(2026-11-19): 演習や台が動いている間でも、差が連絡・設計の文書だけ(コードの置き場とデータに触れない)と coord が確かめた時は、main を進めてよい。コードを含む差は、今までどおり、終わるまで待つ
metadata:
  type: feedback
---

演習(台)が動いている間でも、main..coord の差が文書だけの時は、main を早送りしてよい。

**Why:** 2026-11-19、dkb の EC2 の手元の演習(戻し方の回・1 時間以上)の間に、残りの所の問いの「採用の記録」(bkb の束の道具は main の祖先のコミットから読む)を main に入れる必要が出た。「台が動いている間は main を進めない」は coord の守りとして続けて来た決まりで、dkb(台の持ち主)は「文書だけなら響かない」と答えたが、相手の答えで決まりを緩めずに、利用者に諮った。利用者の答え =「文書だけなら進めてよい」(今回と今後)。

**How to apply:** 早送りの守りで、LOCKS か容器が掛かった時は、`git diff --name-only main..coord` を見る。`knowledge_kb_v8/scripts/`・`accident_kb_v7/scripts/`・`kg_api/`・`scripts/`・`deploy/` に触れる差が 0、データと鍵の類が 0、の時だけ進める(docs/ の下だけ、が目安)。1 つでもコードが在れば、台が終わるまで待つ。進めた時は、報告に「台が動いている間に、文書だけの差で進めた」と書く。第 2 弾の書き手の台(rig_snapshot が本体の git 外の目録を前後で採る)が動いている間の、本体の git 外への書き込みの禁止は、別の決まりで、変わらない。関連 [[no-main-data-writes-during-rig-runs]] [[main-tree-moves-during-run]]
