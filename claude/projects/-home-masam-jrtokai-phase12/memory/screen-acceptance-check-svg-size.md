---
name: screen-acceptance-check-svg-size
description: 画面の受入で「要素がある」だけ見ない — 閉じた expander 内の iframe は幅 0 で描かれ svg が 16×16 の空になる(shirei 探索木(図)・2026-10-09)。viewBox/getBoundingClientRect の実寸と console error 0 を条件にする
metadata:
  type: feedback
---

2026-10-09 決224: 第 3 段の EC2 受入 A5 で Playwright が「svg g.node が 12 個ある」ことだけを見て合格にしたが、閉じた expander の中の st.iframe は幅 0 の文書で mermaid が描かれ、viewBox `-8 -8 16 16`・`translate(undefined, NaN)` error 6 回で、利用者が開いても図は出なかった。console は expander の外だったので正常。

**Why:** 要素の存在は描画の成功と別。隠れた要素は大きさ 0 で処理済みの印だけ付く。

**How to apply:**
- 画面の受入は、対象を**利用者と同じ操作順**(閉じたまま → 開く)で動かし、`svg.getAttribute('viewBox')` と `getBoundingClientRect()` の幅・高さが 0 より大きいこと、ブラウザ console の error が 0 であることを合格条件にする。
- 隠れた場所に iframe で描く物は「見えるまで描かない」作り(startOnLoad false + clientWidth > 0 を待つ)にする(fed の mermaid_html・決224)。
- 関連 [[t0-tree-stage1-ec2-deployed]]。
