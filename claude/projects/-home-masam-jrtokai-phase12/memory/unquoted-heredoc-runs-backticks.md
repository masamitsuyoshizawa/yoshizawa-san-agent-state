---
name: unquoted-heredoc-runs-backticks
description: 値を差し込むために台本の heredoc を引用符なしにすると、本文のバッククォートが命令として走る(read で止まり free が走った・2026-11-21 tkb)
metadata:
  type: feedback
---

連絡文や台本を書く Python を `cat > x.py <<PY`(引用符なし)で作ると、本文の `` `read` `` `` `free` `` などが命令として走る。`read` は組み込みで入力を待って止まり、作業が 300 秒で背景へ移った。`free` は出力が本文に入った。

**Why:** CLAUDE.md の「連絡文はシェルの heredoc で書かない」を、値(コミットと sha16)を差し込むために破った。バッククォートの多い連絡文ほど危ない。

**How to apply:** 台本はファイル書き込みの道具(Write)で書き、値は引数(sys.argv)で渡す。heredoc を使うなら必ず `<<'PY'`。書いた後は、消えては困る語(バッククォートの中の語)を grep -c で数える。関連: [[placeholder-substitution-collisions]]
