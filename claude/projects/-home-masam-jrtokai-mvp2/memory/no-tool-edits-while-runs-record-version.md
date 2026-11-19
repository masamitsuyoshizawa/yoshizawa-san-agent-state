---
name: no-tool-edits-while-runs-record-version
description: 背景の送り(p1_send など)が回っている間は scripts/drawing を書き換えない・回ごとの記録が道具の sha と未コミットの有無をその時点のファイルから書く(2026-11-05・dgr)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-05T06:08:05.442Z
---

背景で回っている送りの道具(`p1_send.py` など)は、回ごとに `common.tool_version(__file__)` で、その時点のディスクのファイルの sha16 と `git_dirty` を記録する。読み込み済みの古いコードで回っていても、記録は新しいファイルを指す。新しいファイルを置いただけでも `git_dirty: true` が残る。

**Why:** f12fc の 3 種の送りの最中に p1_views.py を置き、数秒〜数分の間の回に未コミットの印が残りうる形になった。数の差の決まりも、p1_send に入れると古いコードで回った回の記録が新しい sha を指すので、使う側(p1_output)に入れた。

**How to apply:**
- 送りの最中の新しい道具や直しは、scratchpad の写しで書いて試す(`sys.path` に写しの場所を先に足し、写しのモジュールを先に import する。ほかの道具が自分の場所を `sys.path` の前に足すため)。
- 置いたらすぐコミットする。
- 送りの道具そのものの直しは、送りが終わってから次の図の前に入れる。
- 関連: [[commit-tool-before-writing-handoff]]・[[leak-check-before-tool-commit]]
