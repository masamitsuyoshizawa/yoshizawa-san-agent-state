---
name: remerge-main-before-writing-comms
description: jrtokai-mvp2 のサブセッションは、連絡文・検討記録を書く直前に main を取り込み直し、CLAUDE.md の新しい規則を確かめる
metadata:
  node_type: memory
  type: feedback
  originSessionId: 28bdbe4c-c4d6-4ff3-84bf-fdb3eb0d6845
  modified: 2026-10-28T23:45:47.598Z
---

連絡文や検討記録を書く直前に `git merge main`(と `comms_unread.py`)を回し、CLAUDE.md・docs/01 に足された規則を確かめてから書く。

**Why:** 2026-10-29、dcr は 02:30 に main を取り込んだ後、02:38 に coord が「読取の値は acc が期待を凍結するまで連絡文と docs に書かない」規則(利用者承認)を main に入れた。dcr は知らずに 02:43〜03:09 の連絡文 4 通と検討記録 6 本に構造の数を書き、申告の info(20261029-0312)を出すことになった。規則は数分おきに足される。

同じ日に kgb も同じ型で踏んだ: 02:43 の設計書(スキーマ骨子 改訂 1)の前提の表に、規則の前の dgr・dcr の rep から読取の値(図面の解像度・構造の数など)を写した。08:39 に自分で見つけて是正(b654cc1)し、申告の info 0839 を出した。**読取の側でなくても、他セッションの rep から値を写せば同じ違反になる。**

**How to apply:** 段の区切りで連絡文や記録を書く前に、毎回 main を取り込み直す。読取の側は、acc の凍結の sha が出るまで値を書かず、件数の形・計画・費用だけを書く(値は data/derived/ へ)。kgb など読取でない側も、設計書・連絡文・検討記録には他セッションの値を写さず、出典の参照と数でない様式の記述だけを書く。語彙の台帳(config/vocab_manifest.json)にも語と件数を書かない。書いた後は「数 + 条・表・項目・行・枚・dpi」の grep で確かめる。関連: [[hyperv-port-memo-location]]
