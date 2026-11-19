---
name: neo4j-driver-unknown-label-warning-on-stderr
description: Neo4j の Python ドライバ 6.1.0 は DB に無いラベルを照会すると JSON でない警告を標準エラーへ 1 行出す — 空の DB での最初の 1 回だけ、JSON 行のログを読む試験や工程が壊れる
metadata:
  type: feedback
---

空の Neo4j(立て直した一時 DB・まだ書いていない正本)で、まだ無いラベルを照会すると、ドライバ 6.1.0 が警告を JSON でない 1 行として標準エラーへ出す。ラベルが一度でも作られると出ない。

**Why:** 2026-11-02 に暗黙知の演習の一時 DB を立て直した直後、test_stage2 の W 節だけが JSONDecodeError で中断した(81/82)。2 回目以降は合格し、変更とは無関係だった。一時 DB へ未登録のラベルの読み取りの照会を 1 本投げ、標準エラーへ JSON でない 1 行が出ることで確かめた。

**How to apply:** 空の DB での最初の実行だけが落ちたら、まずこの行を疑う(変更のせいと決めつけない・`json.loads` を包んで読めない行を出す道具で確かめる)。工程のログの規律(stderr は JSON 行だけ)に対しても同じ穴になるので、正本の最初の書き込みの前に、ドライバの通知を切るか工程の側で受けるかを決める。関連 [[tkb-round0-design-state]]。


(索引から移した説明・2026-11-07)
[空の Neo4j でドライバが stderr に非 JSON の警告](neo4j-driver-unknown-label-warning-on-stderr.md)(最初の 1 回だけ JSON 行のログが壊れる)
