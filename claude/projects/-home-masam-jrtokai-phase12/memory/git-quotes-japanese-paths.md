---
name: git-quotes-japanese-paths
description: "git の名の一覧(diff --name-only・ls-tree・status)は日本語の名を \"docs/\\346…\" と引用符と 8 進で包む → 行の頭や名で照らす道具が外れる"
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-20T06:24:05.928Z
---

git は既定(core.quotePath=true)で、日本語を含む道を `"docs/\346\211\213…md"` の形で出す。行の頭が `docs/` かで照らすと、日本語の名の文書を「文書以外」と数える。2026-11-20 に 2 度起きた: (1) 第 2 弾の書き手の領域の照らしが ls-tree を -z 無しで読み、日本語の名で CODE_UNREAD(枝 dkb-r2w で -z に直した) (2) main への早送りの前の「文書以外の差の数」が 2 と出た(中身は手順書と設計書 = 文書だけ・reflog と差で確かめた)。

**Why:** 照らしが外れる向きは 2 つある。安全側(文書を文書以外と数えて止める)なら手間だけだが、名を比べて「同じ」とする照らしでは、引用符の形と元の名が食い違って見落としになり得る。

**How to apply:** git の名の一覧を道具や照らしで読む時は `git -c core.quotePath=false …` か `-z`(NUL 区切り)を使う。早送りの前の文書だけの照らしは `git -c core.quotePath=false diff --name-only main..dkb | grep -vc '^docs/'`。関連 [[main-ff-docs-only-during-rig]]。
