---
name: no-exit-in-bash-guards
description: Bash の道具で「main を取り込み済みか」などの確かめに exit を使うと、実行用のシェルごと終わって時間切れになる。止めるときは exit を使わない
metadata:
  node_type: memory
  type: feedback
  originSessionId: 85f25ae1-06b2-4f77-8b2b-5e740c5782d5
  modified: 2026-10-29T15:12:52.768Z
---

確かめが外れたら止める形にするときは、`{ ...; exit 1; }` を使わず、`&&` の連鎖を切る(`false` を返す・`if` で分ける)か、確かめだけを別の呼び出しにする。

**Why:** 2026-10-30、kgb が `git merge-base --is-ancestor main HEAD || { git log …; exit 1; }` を連鎖の先頭に置いたところ、`exit` が持続するシェルを終わらせ、道具が 120 秒の時間切れで背景に回った(後ろの連絡文の書き込みは走らず、結果は無害だった)。前日には逆に、確かめの結果を見ずにコミットまで進んだこともある。

**How to apply:** 確かめ(main の取り込み・点検の 0 件)は独立の呼び出しで回して結果を見てから、次の呼び出しで書く・コミットする。関連: [[remerge-main-before-writing-comms]]
