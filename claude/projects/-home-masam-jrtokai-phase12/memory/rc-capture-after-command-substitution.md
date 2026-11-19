---
name: rc-capture-after-command-substitution
description: "シェルで終了コードを `echo \"$(…) rc=$?\"` の形で取ると、文の中の先のコマンドの置き換えの終了コードで上書きされる。rc は命令の直後に `rc=$?` の 1 行で取る"
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-18T04:12:18.234Z
---

2026-11-18 dkb: 一括追加 7a の試験を回し直す時に、`echo "$(basename $T) 回 $i rc=$? | $(grep …)"` と書いた。`$?` の前にある `$(basename …)` が先に動き、`$?` は basename の 0 になった。そのため、毎回落ちていた試験を「4 回とも合格・揺らぎ」と報告し、後で訂正した(info 20261118-1311)。出力の中身(R2 の例外で途中で終わる)は、どの回も同じだった。

**Why:** 終了コードの読み違えは、合否を逆にする。出力が同じなのに合否が違う時は、読み方を先に疑う。

**How to apply:** 命令の直後に `rc=$?` を 1 行で置き、その変数を後で出す。試験の合否は、終了コードと、出力の要約の行(何本合格か)の両方で読む。関連 [[verify-zero-counts-before-reporting]] [[negative-test-check-reason]]
