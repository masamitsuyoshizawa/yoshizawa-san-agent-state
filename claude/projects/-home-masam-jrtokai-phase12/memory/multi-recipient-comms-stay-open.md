---
name: multi-recipient-comms-stay-open
description: 宛先が複数の便は閉じない。受領の注記を status 行の末尾に足すだけにする
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7d23b295-de1f-476e-9436-0c42a7039f2a
  modified: 2026-09-21T09:40:41.338Z
---

**受け取った側は、`to:` が複数(または `all`)の便を `closed` にしない。** **`status:` 行の末尾に自分の受領を注記するだけにして、`open` のまま残す。**

**出した側は別。** 自分が出した便が役目を終えたとき(後続の `dec` に引き継いだ等)は、出した本人が閉じてよい。**人の便を閉じるときは、宛先全員が読み終えたことを現物で確かめてから**(2026-09-21 に coord が確認した形)。

**1 人が閉じると、まだ読んでいない宛先の `INBOX.md` と `comms_unread` から消える。** 2026-09-21、私が `info 20260921-1834`(宛先 6)を閉じ、fed・dkb・rev・pla から見えなくなった。同じ便を `open` のまま注記していた bkb と `status:` 行が競合した。全数走査したところ、同じ誤りを他に 3 件していた(`dec 1727`・`dec 1758`・`dec 1828`。いずれも `to: fed, bkb, dkb, ckb, rev, pla`)。

**Why:** 「読んだら閉じる」を宛先の数に関係なく当てていた。CLAUDE.md には「`req` は宛先ごとに分けて出す。1 人が `status` を `closed` にすると、他の宛先の `INBOX.md` から消える」と既に書いてあり、**読む側の作法としても同じことが当てはまる**と気づいていなかった。

**How to apply:**
- 閉じる前に `to:` を見る。複数なら `status: open  # ckb: 読了(…)` の形で注記だけ足す。
- 走査は `to:` が複数か `all` かつ `status: closed` の注記に自分の略号が在るもの。
- 競合したら**両方の注記を残して `open`** にする(相手の受領を消さない)。
- **coord 宛の便に cc で受領の注記を足すのは、coord が閉じた後にする**(2026-09-26 に fed rep 1730・1831 の status 行で dkb の注記と coord の閉じが 2 度競合。coord の依頼・急がない)。cc の受領は会話で伝え、注記は main を取り込んで coord の閉じを見てから。

[[close-own-req-when-answered]](自分発の便は応答が来たら閉じる — こちらは別の話)[[session-comms-protocol]]
