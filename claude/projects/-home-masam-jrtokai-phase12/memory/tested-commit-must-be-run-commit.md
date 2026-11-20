---
name: tested-commit-must-be-run-commit
description: 「版 X で試験が通った」と書くのは、その版で回した時だけ。ファイルを外す・入れ替えるコミットの後は回し直す。字の種類(全角/半角)は字のコードで確かめてから書く
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-15T11:57:18.974Z
---

報告に「HEAD X で試験 N 件が通った」と書くのは、X を出した後に回した結果だけにする。

**Why:** 2026-11-15 rep 0425 で「HEAD 574b4f453 で 29・16・8 が通った」と書いたが、実際に回したのは合成の束 synth-4 を git から外す前の 4ed708fec だった。574b4f453 では試験が外した synth-4 を読んで FileNotFoundError で落ちていた(rev が静的に見つけた)。同じ rep で、正式の見出しの括弧を確かめずに「全角」と書いたが、コードは半角だった。さらに直しの rep で見出しの字数を数えずに「19 字」と書き、実際は 22 字だった。
**How to apply:**
- 試験の版を書く前に、git rev-parse HEAD と回した時の版を照らす。ファイルを外す・入れ替える・束を作り直すコミットの後は、必ず回し直す。
- 「新しい checkout でも通る」と言うなら、git worktree add --detach で別の木に出して回す。
- 全角か半角か、何字かを書く時は、コードの定数を読んで(ord・len)確かめてから書く。人が写す字(dec に書く行など)は特に。
関連 [[estimates-not-labeled-as-measurements]] [[assumed-current-state-without-checking]] [[r351-review-kit-set-dkb]]
- **2026-11-20(bkb・変異の回)**: 変異の回し手は始めに道具を写して固めるが、写した版を出力に書いていなかった。coord に「回した版と書いた版が同じことを後から追えるように」と頼まれ、回し手(作業ツリー knowledge_kb_v8/data/bkb_mutation/mut_run.py)に、**出力の頭に写した道具と試験の sha16・HEAD・始めの時刻、末尾に終わりの時刻**を書く行を足して回し直した。同じ時に、v3 の変異の回を「約 70 分」と確かめずに見込んで書いた(実は 283 個で約 4 分)= 見込みは前の回の出力の時刻から採る([[estimates-not-labeled-as-measurements]])。
