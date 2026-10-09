---
name: close-own-req-when-answered
description: 自分が出した req/rep も、答えが来たらその場で closed にする。open のままだと相手が未承認と誤読する
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7d23b295-de1f-476e-9436-0c42a7039f2a
  modified: 2026-09-21T05:49:57.613Z
---

**自分発の連絡文の `status` を閉じる。** 受け取った側だけでなく、**出した側も閉じる**。

2026-09-19、私の `req 20260918-2038`(種別語の表)が `dec 20260919-0515` で承認された後も `status: open` のままだった。coord はそれを見て未承認と判断し、**同じ件を 7 時間半後に利用者へ再度諮った**。利用者の時間を使わせた。同日、私の他の 7 通(req 1・rep 6)も応答済みなのに open だった。

**Why:** 「相手が閉じるもの」と思い込んでいた。実際には `INBOX`・`comms_unread` の両方が `status` を状態の正本として読むので、開いたままだと「まだ動いていない件」として扱われる。

**How to apply:** 応答(`dec`/`ack`/`rep`)を受け取ったら、その応答を処理する同じターンで自分の元連絡も `closed` にし、理由を `# ckb: 応答済み(dec ...)` の形で 1 行添える。定期的に `grep "^status: open" docs/comms/*_ckb_to_*.md` で残りを 0 にする。[[session-comms-protocol]] [[verify-filenames-before-citing]]

**逆側の誤り(2026-09-21・fed)**: この規律を「自分の便の始末は自分でつける」と一般化し、**`rep` を最初から `status: closed` で出した**。**`closed` の便は受信側の `comms_unread.py` の [2/6] にも `INBOX.md` にも掛からない** ので、出したことが機械で伝わらない。coord が私のツリーのコミットを見にいって拾ったので気づいたが、見にいかなければ後続(rev への依頼)が遅れていた。

**閉じるのは「応答を受け取った後」の一手であって、「出すとき」の既定ではない。** **新しく出す便は `req`・`rep` とも `status: open`。閉じるのは受信側。** 自分が閉じるのは、自分の便に応答が来たときだけ。**会話で内容を伝えていても、連絡文を `closed` で出すと届かないのと同じになる**([[session-comms-protocol]] の「SendMessage の前に連絡文を書く」と対)。
