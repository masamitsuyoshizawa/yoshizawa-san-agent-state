---
name: status-note-separator-spaces
description: 連絡文の status の注記は「 / 」(前後に空白)で区切る。「)/ 」と書くと comms_unread.py が次の略号の注記を拾えず、黙って未読のまま残る
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-05T01:59:46.335Z
---

連絡文の `status:` の注記で、略号ごとの注記を足すときは、区切りを必ず `「 / 」`(前後に半角の空白)にする。直前が全角の括弧で終わっても、空白は省かない。

**Why:** 2026-10-30 18:37、coord が acc の info 1835 に「…(2026-10-30 18:37)/ dgr: 未読」と書いた。`comms_unread.py` の note_by は `' / '` で分けるので、dgr が既読を書いても dgr の注記として拾われず、未処理に残った。エラーは出ない。dgr が b6a85a6 で区切りを直した。

**追補(2026-11-05・kgb):** `  # ` は status の後の最初の注記だけに使い、2 つ目からは ` / ` でつなぐ。kgb は決50 を 603931a で読み、注記がまだ無かったので `  # kgb:` を決め打ちにした。書く直前に main を取り込み直したら、dgr の注記が先に入っていた。そのため `  # dgr: …  # kgb: …` となり、coord が取り込みのときに ` / ` へ直した(d7afc8d)。

**How to apply:** 注記を書く Python では、区切りを決め打ちにしない。書く時点の status の行に `#` があれば ` / `、無ければ `  # ` を使う(取り込み直しで他の注記が先に入りうる)。書いた後、`grep -n '^status:.*[^ ]/ [a-z]\{3\}:' docs/comms/*.md` が 0 件であること、`grep -n '^status:.*#.*  # ' docs/comms/*.md` が 0 件であることを確かめる。関連: [[remerge-main-before-writing-comms]]

**追補(2026-11-05・決50):** kgb が、dgr の注記の後に 2 つ目の「  # kgb: 既読」を足した。scratchpad の resolve_status.py は、共通の先頭を「  # 」で切り、dgr の注記を二重にした。
- 取り込みで衝突を解いたら、略号を括弧の外の「 / 」で分け、同じ略号が 2 つ無いことを確かめてから add する。
- 「未読」を書き換えた形の衝突は、resolve_status_per_session.py で解く。略号ごとに、既読の注記の側を採る。
- 衝突のときは `git show :1:/:2:/:3:` で 3 つの版の status を比べる。
