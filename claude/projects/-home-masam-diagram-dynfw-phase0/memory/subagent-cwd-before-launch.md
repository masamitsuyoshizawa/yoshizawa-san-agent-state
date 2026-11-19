---
name: subagent-cwd-before-launch
description: 観測の担当(補助のエージェント)は起動した時の coord のセッションの作業ディレクトリを引き継ぐ。送信の起動の直前にリポジトリの根であることを確かめる
metadata:
  node_type: memory
  type: feedback
  originSessionId: fdc4c948-da33-4847-b9a6-04212d37500d
  modified: 2026-10-28T05:37:51.315Z
---

補助のエージェント(alpha-observer など)を起動する直前に、coord のセッションの作業ディレクトリがリポジトリの根(`/home/masam/diagram-dynfw-phase0`)であることを確かめる。直前の Bash で `cd proto/...` をしていると、主の作業ディレクトリがそこに移ったままになり、補助のエージェントもそれを引き継ぐ。

**Why:** 2026-10-28 の R3(run pubreg-20261028-1)の 1 起動目の初回で、作業ディレクトリが proto/alpha_contract_bridge のままだったため、起動の文の相対パスの画像を読めず IMAGE_READ_FAILED になった。transport_error として承認の範囲の再試行で回復したが、表の図の再試行の枠を使い切り、20,125 トークンを失った。

**How to apply:** 送信の手順では、起動の前の Bash を絶対パスだけで書き(`cd` で子のディレクトリへ移らない)、最後に `pwd` でリポジトリの根を確かめてから Agent を呼ぶ。R4(54 起動)でも毎回行う。手順書 §4.3 の 4 にも書いた。関連: [[no-parallel-trials-same-worktree]]
