---
name: comms-time-from-date-not-rounded
description: 連絡文の date・ファイル名・注記の時刻は、書く直前の date をそのまま使う・後ろに丸めない(2026-11-04・coord・決43 と決44 で先の時刻を書き dgr に指摘された)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-03T17:19:26.126Z
---

2026-11-04、coord は決43 と決44 で、書いた時の時刻を `date` の実時刻より後ろに丸めた。
- 決43 は date 01:50(コミットは 01:49:36)、閉じた注記は 01:53(コミットは 01:51:23)。
- 決44 は date 02:20(`date` は 02:15・コミットは 02:16:14)。
- dgr が既読の時の実時刻が 02:17 だったことで気づいた。
- df91062 で時刻を正した。ファイル名(0150・0220)は、ほかの連絡文と APPROVED.json から参照されているので変えず、注記で実の時刻を書いた。
- 訂正の注記を書くときにも 02:19 と先の時刻を書きかけた(実は 02:18)。

**Why:** CLAUDE.md §6 は「日付は date の実時刻」と定める。先の時刻を書くと、他のセッションの既読より後に書いたように見え、順序の記録が崩れる。
**How to apply:**
- 連絡文を書く Python の中で `datetime.now()` を取り、ファイル名・`date:`・summary の記録の時刻・status の注記に、同じ値を入れる。
- 手で時刻を書くなら、同じ呼び出しの中で `date` を回し、その値を切り捨てで使う。数分先を見込んで書かない。

関係: [[remerge-main-before-writing-comms]]・[[leak-check-diff-zero-lines]]
