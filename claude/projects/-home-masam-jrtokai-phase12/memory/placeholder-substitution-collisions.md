---
name: placeholder-substitution-collisions
description: 連絡文を置換方式で書くとき、短い置換子(C1・DS・DT)は本文の語(rev の C1・DSL・EC2)まで置き換える。一意な置換子を使い、書いた後に置換子の値の出現箇所を全部確かめる
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-05T09:35:02.196Z
---

2026-10-05 の決180 の rep(20261005-1833)で、Python の置換方式の雛形に置換子 `C1`(コミット)・`C2`・`DS`(便名)を使い、`body.replace("C1", cm1)` などで埋めた。本文の「rev C1」「C1-2」「C2 の測定」「EC2」「DSL」まで置き換わり、rev の条件名と DSL が壊れた(コミット前に grep で見つけ、1 箇所ずつ assert して直した)。同じ日には、計画書への行の挿入で `s[:k] + add` の後ろ `+ s[k:]` を付け忘れて末尾を消した(見出し数の検査で気づいた)。

**Why:** 置換は文字列の一致だけで決まるので、置換子が本文の語の部分列だと黙って本文を壊す。形が正しいので読み返しで見落としやすい。
**How to apply:** 置換子は本文に現れない形(`@@CM1@@` など)にする。埋めた後に、埋めた値(コミットの値・便名)の出現箇所を全部 grep し、意図した数だけかを確かめる。挿入・置換の後は見出し数と長さの増減を assert する。関連 [[scripted-doc-edit-check-structure]] [[verify-filenames-before-citing]]
