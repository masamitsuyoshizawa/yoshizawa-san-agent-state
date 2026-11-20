---
name: main-ff-docs-only-during-rig
description: 利用者の決め(2026-11-19): 演習や台が動いている間でも、差が連絡・設計の文書だけ(コードの置き場とデータに触れない)と coord が確かめた時は、main を進めてよい。コードを含む差は、今までどおり、終わるまで待つ
metadata:
  type: feedback
---

演習(台)が動いている間でも、main..coord の差が文書だけの時は、main を早送りしてよい。

**Why:** 2026-11-19、dkb の EC2 の手元の演習(戻し方の回・1 時間以上)の間に、残りの所の問いの「採用の記録」(bkb の束の道具は main の祖先のコミットから読む)を main に入れる必要が出た。「台が動いている間は main を進めない」は coord の守りとして続けて来た決まりで、dkb(台の持ち主)は「文書だけなら響かない」と答えたが、相手の答えで決まりを緩めずに、利用者に諮った。利用者の答え =「文書だけなら進めてよい」(今回と今後)。

**How to apply:** 早送りの守りで、LOCKS か容器が掛かった時は、`git diff --name-only main..coord` を見る。`knowledge_kb_v8/scripts/`・`accident_kb_v7/scripts/`・`kg_api/`・`scripts/`・`deploy/` に触れる差が 0、データと鍵の類が 0、の時だけ進める(docs/ の下だけ、が目安)。1 つでもコードが在れば、台が終わるまで待つ。進めた時は、報告に「台が動いている間に、文書だけの差で進めた」と書く。第 2 弾の書き手の台(rig_snapshot が本体の git 外の目録を前後で採る)が動いている間の、本体の git 外への書き込みの禁止は、別の決まりで、変わらない。関連 [[no-main-data-writes-during-rig-runs]] [[main-tree-moves-during-run]]

**fed が破った(2026-11-20 12:56)**: dkb の決351 段 3 の台(予告 info 20261120-1252・LOCKS 594 行)の 4 分後に、review_kit の新しい口(コード 5 ファイル + 設計書)で main を早送りした。12:48 に自分の LOCKS の行を足した時は台が無く、その後の main の取り込みに dkb の予告が入っていたのに、早送りの前に見直さなかった。dkb と coord に SendMessage で申告し、台の終わりまで fed の枝だけにした。**How to apply(fed の早送りの手順)**: `merge --ff-only` の直前に、毎回 `python3 scripts/comms_unread.py --me fed` の [4/6] と、取り込んだ差に dkb の `*_info_*lock*` が在るかを見る。台の予約が open なら、`git diff --name-only main..fed` がコードを含む限り進めない。
coord の指示(同日): main を進める前に、LOCKS の開いている行と、直前の 10 分の info を見る。台が動いている間は、docs の外の差が 0 の時だけ進める(coord の守りと同じ形)。dkb の判じ = 段 3 には響かず、main は戻さない(決351 追補 155)。本体ツリーで道具を回す時は `python3 -B` で __pycache__ を書かない。
