---
name: leak-check-diff-zero-lines
description: leak_check.py diff HEAD^1 は既定の base が main なので照らす行が 0 になる・「一致 0」の前に「行 N」が 0 でないことを見る(2026-11-04・coord・決43 の訂正)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-05T02:41:12.351Z
---

`scripts/leak_check.py diff <ref>` は `<base>...<ref>` で足された行を照らし、base の既定は main である。取り込みの後に `diff HEAD^1` と回すと、HEAD^1 の中身はもう main に入っているので照らす行が 0 になる。それでも出力は「一致した行 0」で、終了の値も 0 になる。

2026-11-04、coord は acc の b08e の凍結を取り込んだとき、この形で回した。決43 §3 に「一致した行 0(確定)」と書いたが、実際には何も照らしていなかった。`diff acc --base febed33` で照らし直すと 243 行で一致 0 だったので、090b047 で決43 を訂正した。

**Why:** 漏れの検査は、終了の値と「一致 0」だけを見ると、空の検査を合格と取り違える。
**How to apply:**
- 取り込む差分は `diff <ブランチ> --base <取り込む前の coord の HEAD>` で照らす。取り込む前なら `--base coord` でもよい。
- 出力の「行 N」を読み、N が 0 でないことと、`git diff --numstat` で数えた足された行とおおよそ合うことを確かめてから、「一致 0」と書く。
- 比べる numstat は三点の `git diff --numstat main...<ブランチ>` にする。二点の `main <ブランチ>` は、取り込んだ後に main が進むと main の新しい行を逆向きに含み、合わなく見える(2026-11-05・kgb・rep 1138 で 84 行と 107 行がずれた)。

関係: [[pipefail-before-chaining-checks]]・[[leak-check-new-files-intent-to-add]]・[[merge-alone-before-closing-steps]]

**全体の照らし(2026-11-04・決46)**: 取り込みの検査は足された行だけを照らすので、前からある行は出ない。追跡している全ファイルを `git ls-files -z | xargs -0 ... leak_check.py files` で照らすと、前からある 27 行が当たった。coord が判じて 1 行を一般化し、26 行は既知の当たりとして決46 §3-1 に場所だけを残した。照らす語が大きく増えた時(候補や語彙の新しい版)は、全体を照らし直して §3-1 と比べる。
