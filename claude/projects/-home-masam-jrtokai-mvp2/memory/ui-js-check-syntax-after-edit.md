---
name: ui-js-check-syntax-after-edit
description: ui.js は 1 行に多くの文が並ぶ・行の途中に // の注記を足すと行の残りが消えて画面が動かなくなった・直したら node --check(2026-11-27・dgr)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-26T16:41:21.354Z
---

2026-11-27、部品 0.9.14 の直しで、`scripts/review_ui/ui.js` の 1 行の途中に sed で `// …` の注記を足した。その行は後ろに `}` や `else` が続く形で、行の残りが注記になり、構文が壊れた。selftest は画面の準備を待つ所で時間切れ(30 秒)になり、訳がすぐには見えなかった。

**Why:** ui.js は、1 行に多くの文を詰めて書いてある。行の途中の `//` は、その行の終わりまでを消す。
**How to apply:**
- ui.js に注記を足す時は、行の終わりに置くか、`/* */` を使う。
- ui.js を直したら、selftest の前に `node --check scripts/review_ui/ui.js` を回す。
- 関連: [[no-ui-edits-while-serving]]
