---
name: design-diff-range-by-file-sha16
description: 設計書の差の範囲は「そのファイルの前の sha16 → 今の sha16」で書く(コミットの範囲は間の main の取り込みを含む・coord の指摘 2026-11-20)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-11-20T11:12:23.138Z
---

設計書(1 ファイル)の版の差を rev や coord に示す時は、`<前のコミット>..<今のコミット>` ではなく、**そのファイルの前の sha16 → 今の sha16**(と、どのコミットの版か)で書く。

**Why:** 2026-11-20、fed が設計 6 回目の差を「44741efa6..6e8c58653 の 1 ファイル」と書いたが、2 つのコミットの間に main の取り込みが混ざっていたので、範囲の差の一覧にはほかのファイルも出た。coord が rev への依頼に「設計のファイルの差だけを見る」と書き足して補った。

**How to apply:** rep・req に設計の差を書く時は、例「設計 `docs/設計_….md` sha16 9abceae7e82e2ee2(44741efa6 の版)→ e0480e6f814afd0b(6e8c58653 の版)」の形にする。枝のコードの差は、main との分かれ目からの差のファイルの一覧を添える。関連 [[sha-cut-drops-filename]]
