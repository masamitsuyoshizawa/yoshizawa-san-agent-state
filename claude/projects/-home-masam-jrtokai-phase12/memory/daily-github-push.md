---
name: daily-github-push
description: GitHub(origin)へ最低 1 日 1 回 push する(利用者指示 2026-10-09・元 PC 故障で退避が滞っていた)
metadata:
  type: feedback
---

利用者指示(2026-10-09): **GitHub(origin = github.com:stockmarkteam/yoshizawa-san)へ最低 1 日 1 回は push する。** 各セッションは自分のブランチを区切りごとに、coord は main と全ブランチ(`git push origin --all`・force なし)を 1 日の終わりに。

**Why:** 2026-10-09 の元 PC 故障で、しばらく GitHub へ退避されていなかったことが分かった。ローカルの worktree と手元 DB は失われうる。

**How to apply:** EC2 反映・dec・区切りのコミットの直後に `git push origin coord main`。1 日の終わりに本体ツリーで `git fetch --all` → 各ブランチの ahead を確認 → `git push origin --all`。CLAUDE.md の Git 節に追記済み。関連 [[env-replacement-pc-20261009]]。

**担当(coord 以外)は自分の枝だけを push する。main を origin へ上げるのは coord だけ**(coord の決め・2026-11-22 に確認)。本体ツリーの main の早送り(文書だけの時・[[main-ff-docs-only-during-rig]])と、origin への push は別。上の `git push origin coord main` は coord の手順で、担当は `git push origin <自分の枝>` だけ。
**Why(実例 2026-11-22 04:38)**: dkb が区切りの手順で `git push origin dkb main` を毎回打っていて、fed が取り込んだ確認の道具のコードの入った main を、coord の上げる前の照らしの前に origin へ上げた(中身は rev の合格した版と同じで害は無かった)。
