---
name: session-comms-protocol
description: セッション間連絡は docs/comms 連絡箱(命名規則・ヘッダ・INDEX自動生成・OWNERS/LOCKS/ARTIFACTS)。統合管理セッション(coord)はJR東海用に新設(T0はJR東日本用)。各セッションはworktree分離
metadata:
  type: feedback
---

2026-09-09 利用者承認で導入(docs/検討_セッション間連絡方式_20260909.md・CLAUDE.md「セッション間連絡」節)。
- 連絡は `docs/comms/YYYYMMDD-HHMM_<from>_to_<to>_<type>_<slug>.md`(type req/rep/info/dec/ack、略号 fed=federate改善(本セッション jrtokai-phase12-ff)/dkb=D-KB/coord=JR東海統合管理(新設予定)/user/all)。先頭ヘッダ必須。索引は `python3 scripts/comms_index.py`(`COMMS_ME=fed ... --check` で自分宛open)。
- 作業開始時・コミット前後に自分宛 open を確認。承認事項は needs_user_approval: yes → 判断は dec ファイルに記録。共有資源は LOCKS.md 予約、所有は OWNERS.md(run_fed_vs_dkb.py は dkb 所有・fed は呼ぶだけ)。
- ListAgents では D-KB セッションは見えない(直接メッセージ不可)。T0_統合管理と連動図表 は JR東日本用のため JR東海では使わない。
- **`SendMessage` を出す前に連絡文を書く。順序を固定する。** 2026-09-21 に、同じ日に 8 通の `rep` を作りながら**最後の 2 通で切れた**(版の更新と追測の結果が会話にしか無く、相手が「`docs/comms/` に無い」と指摘して気づいた)。**`SendMessage` を出した時点で「報告した」と感じるため、連絡文と同じ 1 つの行為として扱ってしまう。** 相手は会話で受け取るので作業は進み、**欠落に誰も気づかない**(CLAUDE.md が警告しているとおり)。
- 版混合事故(2026-09-09)の再発防止: 各セッションは `../jrtokai-phase12-<略号>` の worktree で作業(git外データは本体ツリー絶対パス)。本セッションは次の区切りで移行予定。

**Why:** 同一作業ツリーで2セッションが並行し、実行中コードの書き換え(版混合)・共有スクリプトの交互編集・連絡状態の不可視・承認の分散が起きたため。
**How to apply:** 他セッション宛の連絡は必ず docs/comms に作成し INDEX を再生成する。利用者に直接聞くのは承認事項のみ。関連: [[federate-accuracy-improvement]] [[dkg-cause-kg]]

追記(2026-09-09 14:15): coord セッションを新設済み=worktree `../jrtokai-phase12-coord`(ブランチ `coord`)・tmux セッション `coord` 内で claude 稼働(`tmux attach -t coord`、指示は `tmux send-keys -t coord -l "..." ; tmux send-keys -t coord Enter`)。coord 所有の docs/comms は coord ブランチへ即時コミットし巡回毎に main へ ff merge(dec 527e2c5)。origin は git@github.com:stockmarkteam/yoshizawa-san.git(V7メモの URL と異なる)。

追記(2026-09-09 16:30): fed セッションは worktree `/home/masam/jrtokai-phase12-fed`(ブランチ fed)で再起動済み。git外データ(kg_api/kb/index・kg_api/kb/data・knowledge_kb_v8/data)は本体ツリーへのシンボリックリンクで参照し、`.git/info/exclude`(worktree ローカル)で除外(コミット厳禁)。評価スクリプトの BASE は fed ツリー、EV は本体ツリー絶対パス。scratchpad は /tmp/claude-1000/-home-masam-jrtokai-phase12-fed/...。Phase3(fed側)は 4ed1356 でコミット済・非劣化確認(ph3 ホールドアウト+S11)実行中。

追記(2026-09-19 04:45): **記憶ファイルは共有の道具ではない。** `~/.claude/projects/…/memory/` は git の外なので、**merge しても他セッションには届かない**。2026-09-19、coord が「監査台帳に書いたのに他セッションへ渡さず、6 時間後に fed が同じ事故(heredoc の未クォート)を起こした」と申告した。**当方はもっと届きにくい場所に書いていた**(台帳は merge すれば読めるが、記憶ファイルは読めない)。**当方は同じ型を何度も踏みながら「記憶に残した」で済ませていた。**

**以後の運用**: **他セッションに効く規律を得たら `info` で `docs/comms` へ回す**(記憶ファイルに書いて済ませない)。**「効く」の判定**(coord の台帳 (ttt)): **その規律を知らないセッションが同じ事故を起こしうるなら回す。自セッション固有の手順(自分の道具の使い方)なら回さなくてよい。** 何でも回すと `info` が埋まって読まれなくなる。

**遅れの長さは本体ではない。** 6 時間でも 6 分でも同じことが起きる。問題は**「書いた」を「共有した」と同じだと扱っていたこと**。
