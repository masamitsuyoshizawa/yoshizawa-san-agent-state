---
name: local-backup-setup
description: 手元データの D: 退避(決221・2026-10-09): kb_backup.sh の組 S/P/M/T/K・置き場 /mnt/d/jrtokai-phase12-backup/v2・タスクスケジューラ kb_backup_daily 02:00・passphrase は ~/.kb_backup_pass(読まない)
metadata:
  type: project
---

決221(2026-10-09): 案 2(tar + zstd + sha256 目録 + 読み戻し検査・S/P/T は gpg 対称鍵)。
- スクリプト = `scripts/backup/kb_backup.sh`(main・`~/bin/kb_backup.sh` はリンク)。`daily` = S P M T + K incr / `full K` = 月 1 回 / `verify <組>`。
- 組: S 秘密(deploy/secrets・.env*・~/.kb_ec2.env・~/.ssh・~/.aws・codex auth)/ P 個人情報(accident_kb_v7/data 等・docs/eval)/ M 記憶と設定(~/.claude/projects/*/memory・settings・plans・codex config・.gemini)/ T 会話記録(.jsonl・codex sessions)/ K knowledge_kb_v8/data(+ worktree の data)。
- 置き場 `/mnt/d/jrtokai-phase12-backup/v2/<組>/`・`MANIFEST.sha256`・`latest.txt`・`logs/`。状態 `~/.kb_backup/`(指紋・K.snar)。
- passphrase `~/.kb_backup_pass`(利用者が別端末で作成・パスワード管理に保管)。**coord は中身を読まない・会話に出さない。**
- 自動 = Windows タスクスケジューラ `kb_backup_daily`(02:00・`C:\Users\masam\kb_backup.cmd` → wsl.exe)。ログオン時は管理者権限が要り未登録。
- 初回実測: K full 24 GB → 7.9 GB(gzip・10 分 50 秒)・D: の書き込み 64 MB/s。
- 私的リポジトリ = masamitsuyoshizawa/yoshizawa-san-agent-state(daily の最後に agent_state_push.sh)。GitHub の push protection が Antigravity の OAuth トークン(~/.gemini/antigravity-cli)を検出した実例 → S へ移した。
- weekly(日曜 03:00・タスク kb_backup_weekly)= N(Neo4j 4 本 dump・docker は context shared で home の bind mount が効かないので --to-stdout・停止 6〜23 秒)+ G(git bundle)。復元検査 accident-v7 一致(27,267/46,793)。
- 復元手順書 = docs/手順_復元_新PCで0から戻す_coord_20261009.md。D: は 1 TB SSD(2026-10-09 入替)。D: が stale のときは `wsl.exe -d Ubuntu-oldpc -u root -- mount -t drvfs D: /mnt/d`。
- prune(weekly の最後・残す最新の sha を照らしてから消す)・restore-test(月 1 日 04:00 のタスク kb_backup_monthly・初回 2026-10-10 全合格・7 分)。未了 = ログオン時タスクだけ(管理者権限・任意)。
- 関連 [[daily-github-push]] [[env-replacement-pc-20261009]]。
