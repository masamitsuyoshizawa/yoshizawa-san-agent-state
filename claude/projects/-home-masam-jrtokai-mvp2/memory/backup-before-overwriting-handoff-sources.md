---
name: backup-before-overwriting-handoff-sources
description: 渡した kgb_input の derived_from が指す出力(rg3・rg5・read_v0・多数決など)を同じ場所に書き直す前に、前の版を _prev/ に写して SHA256SUMS を置く
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-01T15:49:43.426Z
---

渡した成果物(kgb_input など)の `_meta.derived_from` が sha16 で指す出力を、同じ場所に書き直す前に、前の版を `_prev/<道具の版>/` に写し、`SHA256SUMS` を置いて照らす。

**Why:** 2026-11-02、dgr が段 D で `rg5_regions.json` を前の版を写さずに書き直し、rep 0006 の版の kgb_input の `derived_from` の sha16 が今のファイルと合わなくなった(coord が決40 で指摘)。出力に書いた時刻(generated_at)があるので、同じ道具で回し直しても sha16 は戻らず、その版は「出所の 1 つが失われた版」になった(info 0049)。R-G3 のとき(決35)は写していたのに、R-G5 の直しでは忘れた。

**How to apply:** 道具を回して git 外の出力を書く前に、「この出力を指す渡し済みの物があるか」を見る。あれば先に写す。関連 [[commit-tool-before-writing-handoff]]・[[verify-unfrozen-drawing-by-boolean]]。
