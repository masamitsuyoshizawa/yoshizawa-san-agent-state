---
name: merge-only-main-not-peer-branches
description: 自分の枝には main だけを取り込む。ほかの担当の枝(origin/rkb など)は取り込まず、便は git show で読む(coord の指示 2026-11-24)
metadata:
  type: feedback
---

自分の枝(dkb)には **main だけ**を取り込む。ほかの担当の枝は、便を読むためでも取り込まない。便の現物は `git show <コミット>:<道>` か `git fetch` の後の `git show origin/<枝>:<道>` で読む。

**Why:** 2026-11-24 13:12 に、rkb の便を読むために `git merge main origin/rkb` をした。その結果、rev が要是正とした rkb の道具(r1_timeshape.py 0763faf17 の版)が枝 dkb に入った。coord は dkb の枝をそのまま取り込めなくなり、連絡箱の分だけを選んで取り込んだ(coord 13:20)。main を後で取り込んでも、git は枝の側の変更を残すので、版は戻らない(info 20261124-1319)。戻すコミットを作ると、rkb の版が main に入った後に dkb の枝を取り込んだ時、rkb の版を古い版へ戻してしまう。

**How to apply:** CLAUDE.md の「そのブランチを merge して現物を読む」は、git show で現物を読む事で満たす。枝 dkb の r1_timeshape.py と試験は、rkb の版(か後の版)が main に入るまで main と違う。それまで coord は dkb の枝から連絡箱の分だけを選んで取り込む。[[daily-github-push]]・[[session-comms-worktree]]
