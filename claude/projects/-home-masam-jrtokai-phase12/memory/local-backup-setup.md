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
- 未了: 私的 GitHub リポジトリ(M の第 2 の場所)・Neo4j dump・git bundle・prune・月 1 回の復元検査・復元手順書(段 4)。
- 関連 [[daily-github-push]] [[env-replacement-pc-20261009]]。
