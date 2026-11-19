---
name: truth-check-vacuous-all
description: 真偽だけの照らしで all() が照らす物 0 件のまま真になった・鍵の名を確かめ、照らした数が 0 でないことも出す(2026-11-08・dgr・rep 2250 §5 で訂正)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-08T13:50:44.718Z
---

真偽だけを出す照らし(`all(... for r in rows if "read" in r)`)で、答えの行に無い鍵の名(`read`、正しくは `read_v1_sha16`)を使った。照らす行が 0 件になり、`all([])` が真を返した。その「真」を coord に知らせた(2026-11-07 の 8753 の立て直し)。正しい鍵で照らし直すと結論は同じだったが、照らしとしては中身が無かった。

**Why:** 値を出さずに真偽だけを出す照らしでは、空の照らしと本当の一致が、出力の上で見分けられない。

**How to apply:**
- 真偽の照らしには、「照らす物が 1 件以上ある」の真偽を並べて出す(例: `len(xs) > 0 and all(...)`)。
- 鍵の名は、照らす前に、書き手の道具のコード(行を作る所)で確かめる。
- 関連: [[verify-unfrozen-drawing-by-boolean]]・[[leak-check-diff-zero-lines]](0 行でも「一致 0」と出る件と同じ形)
