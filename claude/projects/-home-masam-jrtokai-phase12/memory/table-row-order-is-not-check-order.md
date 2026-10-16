---
name: table-row-order-is-not-check-order
description: 照合表の行の並びを照らす順と読まない・hash が別の hash を含むと照らす順が結果を決める(決263-A 改訂 11→12)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-16T14:34:06.637Z
---

照合表の行の並びを「照らす順」と読み、fed 設計書 改訂 11 に誤った順(待機点 → 計算し直した束 → 表示)を書いた。bkb の bundle_sha は view_sha を欄として含むので、表示だけ差し替えても束の sha が変わり、表示を後に照らすと期待の BUNDLE_VIEW_CHANGED/VIEW_MISMATCH に届かない。bkb の実装は正しく表示を先に照らしていた。bkb の偽物でなく本物(git show で写した ingest_bundle.py)で試験を回して気づいた(2026-10-16・fed 改訂 12 db9a804f78f665b8・bkb 改訂 15 f47905b975018c08 で順を明記)。

**Why:** 表は「何を何と照らすか」しか言わない。ある sha が別の sha を含む(束 ⊃ 表示)とき、外側を先に照らすと内側の差し替えも外側の理由コードで止まり、内側の負例が成り立たない。

**How to apply:** 照合の設計では順を表と別に 1 行で明記する。含む関係のある sha は内側を先に照らす。負例は「順を入れ替えた変異で落ちる」ことで順を試験に縛る。他者の計算は作法を写した偽物だけでなく本物でも回す。関連 [[artifact-self-report-not-trusted]]・[[spec-unverified-until-implemented]]
