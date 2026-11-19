---
name: grep-hardcoding-before-running-copied-tools
description: 姉妹 PJ から写した道具(comms_*.py 等)は、PJ 名・略号・出力先の決め打ちを grep してから回す。mvp2 立ち上げ時に phase12 の INBOX 7 つを上書きした事故あり
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-10-28T23:02:53.005Z
---

姉妹 PJ(jrtokai-phase12 など)から道具を写したら、**回す前に** PJ 名・セッション略号・作業ツリーの場所・出力先の決め打ちを grep して直す。

**Why:** 2026-10-29、jrtokai-mvp2 の立ち上げで phase12 から写した `scripts/comms_dispatch.py` を直す前に回し、phase12 の 7 つの作業ツリーの INBOX.md を上書きした(phase12 の coord が復元済み)。rev・agy のように略号が両 PJ で同じだと、出力先が他 PJ でも気づきにくい。mvp2 では 2bf900f で `REPO = "jrtokai-mvp2"`(INBOX は `../<REPO>-<略号>/INBOX.md`)に直してある。

**How to apply:** 写した道具の初回実行の前に `grep -nE 'phase12|<他PJの略号>|/home/' scripts/*.py` を見て、`REPO`・`SESSIONS`・`BRANCHES`・`TOPICS` の値と書き込み先の組み立て(`os.path.join(parent, REPO-略号)` 等)を確かめる。コメントや試験の中の他 PJ の略号は害がない。関連: [[remerge-main-before-writing-comms]]
