---
name: leak-check-new-files-intent-to-add
description: leak_check.py worktree は追跡の対象だけを見る・新しいファイルは git add -N してから回す・coord と同じ形は diff <ブランチ> --base main
metadata:
  node_type: memory
  type: feedback
  originSessionId: 85f25ae1-06b2-4f77-8b2b-5e740c5782d5
  modified: 2026-10-30T17:52:44.151Z
---

`scripts/leak_check.py worktree` は git が追跡しているファイルだけを見る。まだ追跡していない新しいファイル(新しい連絡文・検討記録・Schema)は検査に入らない。

**Why:** 2026-10-31、kgb が rep 0214 を書いて worktree の検査を回し、一致 0 とした。coord の diff の検査では同じ rep の 3 行が当たった(文法の言い回しで、漏れではなかった)。新しいファイルを `git add -N` していなかったためと、coord が決28 §3 で指摘した。

**How to apply:** コミットの前に、新しいファイルを先に `git add -N <path>` してから `set -o pipefail` で worktree の検査を回す。coord と同じ形で見るには、コミットの後に `diff <ブランチ> --base main` を回す(`diff main` だけでは行 0 になり、何も見ていない)。検査の「行」の数が 0 のときは、検査が空振りしていないかを疑う。関連: [[pipefail-before-chaining-checks]]
