---
name: ai-hub-enabled-dec178
description: mvp2 の ai-hub(rev・agy の窓への自動の知らせ)は 2026-11-21 13:01 から本番・req の出し方の決まりと、当たった落とし穴(決178)
metadata:
  node_type: memory
  type: project
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-21T04:09:56.730Z
---

2026-11-21、利用者の指示で、mvp2 に ai-hub(道具 1.0.0・持ち主は jrtokai-phase12-coord・`~/ai-msg-lab`)を入れ、13:01 に本番にした。coord が `INBOX.md` に req を載せると、見張りが rev(15 番の窓)と agy(16 番の窓)に、決まった 1 文を打つ。試しの req で、rev と agy に打たれ、rep が返った(info 20261121-1308)。

- 入れた物: `.ai-hub.json`(`{"enabled": true}`)・`AGENTS.md`(境界 = 自分のツリーの追跡のファイルと INBOX.md だけを読む)・CLAUDE.md §6・SESSIONS.md・docs/04 改訂 3・`comms_dispatch.py` の 1 行。
- rev・agy → coord の向きは自動にしていない(見張りは「coord へ打てない」と保留にする・害なし)。
- Antigravity は、作業ツリー直下の `AGENTS.md` を自分で読み込む(agy の rep で確定)。
- 残り: rev と agy の指摘 4 点(AGENTS.md と INBOX の手順の直し)を、利用者に諮っている(2026-11-21 13:10)。

**Why:** 入れる前に `comms_dispatch.py` を回すと、済んでいた req 8 本が open のまま載る所だった。また、見張りは初めの起動で、既に載っている req を済みとして記録するので、先に置いた試しの req には打たれなかった。

**How to apply:**
- rev・agy に req を出す手順: req をコミット → main へ ff → `git -C ~/jrtokai-mvp2-<rev|agy> merge --ff-only main` → `python3 scripts/comms_dispatch.py rev agy`(書かずに、載る req を見る)→ `--write-inbox rev agy`。`ai-hub status` で見張りが動いている事を先に見る。
- 返事は、2 つのツリーの未コミットで見る。漏れの検査は、そのツリーで `git add -N` してから coord の `leak_check.py worktree`。代行コミット → coord に merge(status の競合は `data/private/coord_tools/resolve_status2.py`)→ INBOX を作り直す(0 件に戻す)。
- req の `re:` は、その件の直前の便だけ(広い dec を足すと、スレッドの根が古い便に集まり、往復の上限 3 回に早く当たる)。
- 設定を変えた時は、`ai-hub stop watcher` → `ai-hub watch-only`(窓の起こし直しは要らなかった)。
- 往復 3 回か 1 日 30 回で止まった時は、利用者に諮ってから `ai-hub reset`。
- AGENTS.md の境界は文書の指示で、道具による止めではない([[review-ui-common-parts]] の agy_check.py の隔離とは別の口)。
