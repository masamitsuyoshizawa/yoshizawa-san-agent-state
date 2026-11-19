---
name: pipefail-before-chaining-checks
description: 検査の道具の出力を tail などに通して && でコミットにつなぐときは、先に set -o pipefail(終了の値が消えて、一致ありのままコミットした件)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-10-30T07:27:09.466Z
---

検査(`scripts/leak_check.py` など)を `| tail -1 && git commit …` の形でつなぐときは、コマンドの初めに `set -o pipefail` を置く。または、検査を別の呼び出しにして、結果を見てからコミットする。

**Why:** 2026-10-30、jrtokai-mvp2 の coord が決22 をコミットするときに、漏れの検査が「一致した行 2」を返した。しかし `tail -1` の終了の値 0 で && が進み、そのままコミットして main に進めた。当たったのは文法の言い回しで漏れではなかったが、本当の漏れでも同じく通っていた。

**How to apply:** 終了の値で止めたい検査を、パイプで短くして && でつなぐ前に、`set -o pipefail;` を付ける。関連: [[no-exit-in-bash-guards]]・[[grep-hardcoding-before-running-copied-tools]]

**再発(2026-11-14・dgr)**: pipefail は付けたが、検査とコミットを `;` でつないだので、検査が一致 2 でもコミットへ進んだ(rep 1420 の要約の 1 文)。`;` は終了の値を見ない。検査の後は必ず `&&` でつなぐか、検査を別の呼び出しにして結果を見てから進む。取り込みの前なら、直した中身でコミットを置き換えた(dgr の外に出ていないことを `git branch -a --contains` で確かめてから)。
