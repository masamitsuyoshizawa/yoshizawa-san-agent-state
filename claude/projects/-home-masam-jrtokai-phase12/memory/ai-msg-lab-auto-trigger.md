---
name: ai-msg-lab-auto-trigger
description: AI セッション間の自動トリガー(ai-hub・ai-msg-watcher)の試作 ~/ai-msg-lab(2026-11-03)・tmux 前提・24 の窓への統合は未承認
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-11-20T06:20:55.496Z
---

**版 1.0.0(2026-11-20・決363・lab コミット 077861a)**: 利用者の指示で汎用版にした(計画は Plan Mode で承認)。足した物 = `ai-hub wrap --check DIR` / `wrap --dir DIR --script F`(窓を起こす仕組みから呼ぶ入口・rc 0 / 11 = 呼び手が起こす / 21)・`.ai-hub.json` の `"enabled": true` で有効(無ければ何も変わらない・対象は既定で codex と agy だけ)・窓を閉じるとエージェントも止まる・再起動の後の追い打ち(2 回目の起動から snapshot し直さない)・起動の猶予 20 秒と打鍵の守り 20 秒・見張り 1 本のロック・`stop`/`doctor`/`log`/`version`。文書 = lab の `docs/導入の手順.md`・`運用の手順.md`。ai_agent_environment への指示文 = `~/ai_agent_environment/docs/REQUEST_FROM_PHASE12_COORD_AI_HUB_LAUNCH.md`(改造はそこの Antigravity・利用者が渡す)。試験 = 単体 114・変異 41/41・通し wrap_e2e 40・通しの変異 8/8。**残り**: phase12 の有効化(`.ai-hub.json`・CLAUDE.md 2 項・AGENTS.md)は利用者の合図の後 → 立ち上げの受入(§7)→ mvp2・dynfw へ依頼(各 1 日のドライラン)→ 任意で coord の窓も tmux に。関連 [[tmux-34-pitfalls]]。

利用者依頼(2026-11-03)で、連絡箱方式の「新しいメッセージを読んで処理して」「返信を確認して」の手の打ち込みを自動化する道具を試作した。リポジトリ `~/ai-msg-lab`(git・リモートなし・worktree coord/rev/agy)。`~/.local/bin/ai-hub`・`ai-msg-watcher` は lab の bin への symlink。

- 見張るのは外部エージェント(codex・agy)のツリーだけ: (a) INBOX.md に新しい req → そのエージェントへ (b) そのエージェント差出人の新しい連絡 → coord へ。Claude 差出人は打たない(SendMessage 前提)。終端 = closed・to none・info/dec/ack。
- ID = `ai-<本体ツリー名>-<md5 6 桁>`(worktree 共通)。状態 `~/.local/state/ai-msg/<ID>/`。
- 守り: エージェントが動いている窓だけ・**選択の確認の表示中は打たない**(V4 で Claude Code の信頼の確認の既定が「No, exit」・作業中の許可の確認も出ると判明)・3 往復で止めて NEEDS_HUMAN・間隔 60 秒・1 日 30 回・pause。
- 試験: V1 53/53・変異 16/16・V3 tmux 20/20(守りを外す変異で再現)・V4 本物の claude/codex/agy で 2 往復が ack で止まった。

**Why:** 今の 24 の窓(Windows Terminal・`~/ai_agent_environment`)は tmux の外なので、そのままでは打てない。

**How to apply:** 実プロジェクトへの適用(起動の仕組み `Build-AgentBashLines` の tmux 化・CLAUDE.md/AGENTS.md への終端の規約の追記)は利用者の判断待ち。まずドライランを数日回して手の打ち込みと照らすのが推し。関連 [[session-comms-worktree]]・[[rev-session-codex]]・[[gate-accepted-shapes-from-raw-response]]。

**phase12 のドライラン(利用者指示 2026-11-03)**: 2026-11-03 20:52:54 から tmux のセッション `aimsg-dryrun-phase12` で `ai-msg-watcher --project ~/jrtokai-phase12-coord --dry-run`(lab cec5d6f)。ログ `~/.local/state/ai-msg/ai-jrtokai-phase12-954629-dryrun/watch.log`。数日後に、ログの DRY-RUN の行を (1) coord の会話で利用者が「rev の結果を確認」と打った時刻(Claude の会話の記録)と (2) rev・pla・agy の起動の時刻(~/.codex/sessions 等)と照らして、取りこぼし・余計な打ち込み・遅れを数える。WSL の停止・PC の切替で止まる。

**ドライランの結果(2026-11-11 に coord が確認)**: 見張りは 11-05 21:50 が最後の記録。11-06 00:17 に Codex が全部起動し直しているので、その頃の WSL の停止で tmux ごと止まった(推定)。以後は動いていない(tmux のサーバー無し)。
- 実際の出来事は 11-05 の rev だけ(agy・pla は 0 件)。打つはずだった 6 回(rev へ req 3・coord へ rep 3)は、ファイルが書かれてから約 30〜60 秒で検知した。info と他の差出人の連絡は正しく打たなかった。
- **欠陥**: 往復の上限(3 往復)を「スレッドの根」で数えるため、長く続く dec(決350 の根 1832)の下の別々の点検が全部同じスレッドになり、17:40 以降は全件が人の判断へ回った。決352 のように 1 つの dec に点検が何便も続く使い方では、すぐ止まる。数える単位を req ごと(re: の鎖)にするか、人の打ち込みで数えを戻す直しが要る。
- 人の打ち込みの時刻との照らしは未実施。

**直しと再開(2026-11-11・利用者の指示「1 と 2 を進めて」)**: lab c2d946b。往復の上限は「人を挟まない」往復で数える(coord の Claude の会話の記録 `~/.claude/projects/<作業場所>/*.jsonl` に人の打ち込みが在れば、それより後だけ数える・記録は後ろから読む・見張りの 2 文・isMeta・道具の結果は人と数えない)。V1 合格・変異 22/22・V3 合格。照らしの道具 `tools/compare_day.py`(Codex の人の打ち込みは `~/.codex/history.jsonl` + `state_5.sqlite` の threads.cwd・11 月から sessions/ の rollout は無い)。11-11 の rev 10 件: 打つはずから人までの遅れの中央値 30 分・最大 437 分(夜)・新しい規則で止まる 0 件(人が他の話を一切しなければ 4 件)。11-05 は旧規則 3 件 → 0 件。ドライランは 17:49 に tmux `aimsg-dryrun-phase12` で起こし直した(前回の状態は `...-dryrun-20261103` に残した)。WSL が止まると止まる。


(索引から移した説明・2026-11-07)
- [AI セッション間の自動トリガー試作](ai-msg-lab-auto-trigger.md) — ~/ai-msg-lab・ai-hub/ai-msg-watcher・tmux 前提・V1〜V4 合格・phase12 でドライラン中(tmux aimsg-dryrun-phase12・11-03 20:52〜数日)・24 の窓への統合と規約の追記は利用者の判断待ち

**本番の立ち上げ(rev だけ・2026-11-13 16:50 頃・利用者の承認「進めてください」)**: 11-13 のドライランは rev への req 5 件・rev の rep を全部 30 秒〜2 分で検知・余計な打ち込み 0・止めた 0。coord が tmux `ai-jrtokai-phase12-954629` を窓 rev(bash・rev のツリー)と watcher(本番・`ai-msg-watcher --project /home/masam/jrtokai-phase12`・lab c2d946b)だけで作った(`ai-hub --start` は全部の窓を起こすので使わない)。利用者が rev の Codex を終了し、`ai-hub open rev` でこの窓に入って Codex を起動し直す。窓が無い coord・pla・agy への打ち込みは保留のまま(`_guard` が「窓が無い」で打たない)。起動時の snapshot は既存の req に打たないので、最初の 1 回は手で打つ。ドライランの tmux(aimsg-dryrun-phase12)は並べて残した。coord の窓の tmux 化は決351 の演習の区切りの後。


(索引から移した説明・2026-11-20)
- [AI セッション間の自動トリガー試作](ai-msg-lab-auto-trigger.md) — ~/ai-msg-lab・V1〜V4 合格・往復の上限を人を挟まない往復で数えるよう直した(c2d946b)・ドライラン再開 11-11 17:49・統合は利用者の判断待ち
