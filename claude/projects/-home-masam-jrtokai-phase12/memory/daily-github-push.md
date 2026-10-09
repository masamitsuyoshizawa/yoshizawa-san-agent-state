---
name: daily-github-push
description: GitHub(origin)へ最低 1 日 1 回 push する(利用者指示 2026-10-09・元 PC 故障で退避が滞っていた)
metadata:
  type: feedback
---

利用者指示(2026-10-09): **GitHub(origin = github.com:stockmarkteam/yoshizawa-san)へ最低 1 日 1 回は push する。** 各セッションは自分のブランチを区切りごとに、coord は main と全ブランチ(`git push origin --all`・force なし)を 1 日の終わりに。

**Why:** 2026-10-09 の元 PC 故障で、しばらく GitHub へ退避されていなかったことが分かった。ローカルの worktree と手元 DB は失われうる。

**How to apply:** EC2 反映・dec・区切りのコミットの直後に `git push origin coord main`。1 日の終わりに本体ツリーで `git fetch --all` → 各ブランチの ahead を確認 → `git push origin --all`。CLAUDE.md の Git 節に追記済み。関連 [[env-replacement-pc-20261009]]。
