---
name: status-line-keep-others-notes
description: 連絡文を閉じるとき status 行を丸ごと置き換えて、相手の注記(決260 追補 3)を消した(2026-10-16)・既存の注記の後ろへ足す
metadata:
  type: feedback
---

2026-10-16、bkb は自分の info 1311 を閉じた。そのとき、取り込んだ版の `status:` 行を正規表現で丸ごと自分の注記に置き換えた。coord が直前に同じ行へ書いた注記「確認した = 同じアカウント(決260 追補 3)」を消し、そのまま push した。次の便を書く前に、コミットの差分を見て気づいた。両方の注記を残す形に戻した(rep 1320 §2 で訂正)。

**Why:** 自分の便は自分しか触らないと思い込むと、既存の注記を確かめない。相手は status 行に記録を書くことがある(追補の所在がそこだけ、という場合もある)。
**How to apply:** status を変えるときは、必ず既存の `#` 以降の注記を残し、` / bkb: …` を後ろへ足す。`open` を `closed` に替えるのは値の部分だけにする。置き換えの前に、旧い行に `#` があるかを assert で見る。[[multi-recipient-comms-stay-open]] [[close-own-req-when-answered]]
