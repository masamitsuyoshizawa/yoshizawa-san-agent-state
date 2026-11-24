---
name: screenshot-with-temp-store-not-real-app
description: 確かめの画面を写す時に配りの道具の App(本当の答えの置き場)で開き、見た記録の行を 21 足した・写しは一時の置き場の rui.App で開く(2026-11-24・dgr)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-24T12:57:50.571Z
---

2026-11-24、配線の第 5 回の問いの画面を写す時に、`p1_s5_reps.make_app(sid)` で開いた。この App は本当の答えの置き場(answers.jsonl)を使うので、部品が見た記録・位置・時間の行を書き込み、利用者の記録に dgr の行が 21 混ざった(答えの行は無し)。行は消さず、行の番号を git の外に記して、読む時に除いた。

**Why:** 部品は、問いを開くだけで seen・pos・time の行を置き場に書く。
**How to apply:**
- 画面を写す・数える時は、`rui.App(forms, <一時の置き場>/a.jsonl, files=...)` で開く(数えの道具 cover_report・roundtrip_report と同じ形)。配りの道具の make_app をそのまま使わない。
- 混ざった時は、行を消さず、行の番号と訳を git の外に記し、rep に書く。
- 関連: [[no-ui-edits-while-serving]]
