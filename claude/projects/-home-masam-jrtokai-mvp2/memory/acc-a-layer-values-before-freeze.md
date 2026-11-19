---
name: acc-a-layer-values-before-freeze
description: 凍結の前の図の acc の A 層の値(線の比・検出の内訳など)は docs・連絡文に書かない・検出の有無と件数の形だけ(逆の向きの独立)
metadata:
  node_type: memory
  type: feedback
  originSessionId: b54d92f6-694f-48da-9c1b-bc1712afcb52
  modified: 2026-10-30T14:46:37.417Z
---

凍結の前の図(期待を info で出す前)について、acc の A 層の値(枠の線の広がりの比・どの隅でセルを検出したか・節の数から分かる表の数など)は、検討記録・連絡文に書かない。書くのは「検出できたか」と件数の形だけにする。

**Why:** dgr が読める所に期待の材料が出ると、dgr が自分の検出をそれに合わせられ、逆の向きで受入の独立が弱まる(2026-10-30・coord の指摘・acc の検討記録 §23 で b08e の縦の線の比と他の図の比を書いた件。比は照合の値ではないので直さなくてよいとされた)。

**How to apply:** 道具の不具合を診るときも、画面や記録に出すのは真偽と「閾値に届かない」程度にとどめる。受入の独立の本体の規則(読取の値を凍結まで書かない)は [[remerge-main-before-writing-comms]] と CLAUDE.md §4。件数の内訳の扱いは [[check-counts-by-type-leak-acceptance]]。
