---
name: leak-check-before-tool-commit
description: 道具のコミットの前にも漏れの検査を回す — 区切りのコメント「# ---------- 視力表 ----------」が原本の 8 字に当たり、coord が取り込みを止めた(2026-11-05・dgr・1668ddc → 125fc92)
metadata:
  type: feedback
---

道具(scripts/)をコミットする前にも、`leak_check.py worktree`(新しいファイルは先に `git add -N`)か `diff dgr --base main` を回す。連絡文や docs のときだけ回すのでは足りない。

**Why:** dgr が synth_drawing.py を漏れの検査なしでコミットした。節の見出しのコメント(漢字 1 字とハイフン 7 字)が、原本の文字層の 8 字の類に当たり、coord が取り込みを止めた(決22: 一致を残したまま取り込まない)。顧客の情報は含まない誤検出だったが、直しのコミットと知らせが 1 往復増えた。

**How to apply:** 区切りのコメントは `# 見出し` だけにし、記号の長い連なり(ハイフン・イコール)を字に続けない。表の罫や区切りの形は、原本の文字層にもありうる。[[leak-check-new-files-intent-to-add]]・[[pipefail-before-chaining-checks]] と同じ手順で回す。
