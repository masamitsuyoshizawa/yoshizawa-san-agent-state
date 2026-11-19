---
name: report-counts-from-runs-used
description: 報告の「採った回」の数は出力の runs_used から数える・送りの道具の status の数で代えない(rep 1532 で claude 5 と書き、出力は claude 6・codex 1 だった・2026-11-05・dgr)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-05T13:34:24.656Z
---

`p1_send.py status` の「通った / 数の差 1 割以内で採る」は、項目ごとに「通った回が 1 つでもあるか」で数える。`p1_output` は決57 の判断 4 のとおり「使える回のうち最初の回」を使う。1 回目が 1 割以内の差で、送り直しが通った項目は、status では「通った」、出力では数の差の回になる。

**Why:** rep 1532(f12fc の D7)に status の数を写し、claude 5(codex は無し)と書いた。出力が使った数の差の回は claude 6・codex 1 で、rep 2234 §5 で訂正した。

**How to apply:**
- 報告の数は、出力の `_meta.runs_used[]`(`count_gap` と `rep`)から数える。
- 送り直しのある図では、status と出力の数が違いうると思って照らす。
- 関連: [[no-tool-edits-while-runs-record-version]]・[[verify-unfrozen-drawing-by-boolean]]
