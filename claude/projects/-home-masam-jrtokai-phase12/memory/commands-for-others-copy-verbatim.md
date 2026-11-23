---
name: commands-for-others-copy-verbatim
description: 他の者が打つ命令は、試した字(別紙の行・道具の選び)から字をそのまま写す。縮めて書くと道具が引数の誤りで止まる(2026-11-23 port-check --want listen)
metadata:
  type: feedback
---

coord が打つ命令の一式を書く時、別紙の W1-2 の `--want listening` を、表の中で `--want listen` と縮めて書いた。道具は argparse の選び(free・listening)で引数の誤り rc 2 に止まり、票を作らず、coord は「票を見てほしい」と書いてから、票が無い事に気づいた(2026-11-23 20:2x・決365 段 0)。

**Why:** 打つ人は字のとおりに打つ。表の中の省略は、読む人には意味が通っても、道具には通らない。

**How to apply:** 他の者に渡す命令の行は、試した版の別紙か、道具の引数の定義(choices)から字を写す。「同じ形で」と書く所も、選びの値は字で全部書く。渡す前に、引数の解析器(`parse`)に通せる行は通す。関連 [[procedure-python-must-match-tested-python]] [[verify-filenames-before-citing]]
