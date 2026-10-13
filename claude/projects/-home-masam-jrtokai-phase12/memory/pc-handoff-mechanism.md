---
name: pc-handoff-mechanism
description: 2 台の PC(元 PC と代替 LAPTOP-15FQI8C9)の切替 — handoff.json(agent-state リポ)の active_host で門・pc_handoff_out/in・export は 2 ディストロ(dockerd は Ubuntu 側)・手順書 20261013
metadata:
  type: project
---

2026-10-13 に用意(手順書 `docs/手順_元PCへの移行と2台切替_coord_20261013.md` 改訂 1・決233 で H-1〜H-4 承認・元 PC では Ubuntu/Ubuntu-docker の名・C: 348 GB 空きなので 2 回に分けて import・USB SSD の文字が変わるので `~/.kb_backup.conf` に KB_BACKUP_DEST)。

- **dockerd は作業ディストロ `Ubuntu-oldpc` でなく別ディストロ `Ubuntu` に在る**(shared-socket.conf・context `shared`)。Neo4j 4 本の volume も `Ubuntu` 側。`Ubuntu-oldpc` だけ export しても DB は運ばれない。
- 切替の記録 = 私的リポジトリ `yoshizawa-san-agent-state` の `handoff.json`(active_host / heads / backups)。`handoff_lib.sh` の門で、活動中でない PC の `kb_backup.sh daily` と `agent_state_push.sh` は exit 2(否定例を実測)。
- 道具: `scripts/backup/pc_handoff_out.sh --check|--run`、`pc_handoff_in.sh --status|--claim|--restore T U K N`、`win/pc_handoff_export.ps1`・`pc_handoff_import.ps1`(UTF-8 BOM 必須・`"$t:"` は `${t}:`)。組 U(git 未追跡・gpg)を新設。
- 退避タスクが 10-11・10-12 に走らなかった原因 = 電池の制限と StartWhenAvailable 無し(XML を直した版をリポに置いた)。
- `cd` を含む複合コマンドで cd が効かず `cp ../` が home に落ちた(絶対パスで書く)。

- **元 PC(DESKTOP-OCT2024・2026-10-13 18:31 claim)**: 作業ディストロ `Ubuntu`・docker は **Docker Desktop**(Ubuntu-docker は使わず unregister)・USB SSD は **F:**(`~/.kb_backup.conf` KB_BACKUP_DEST=/mnt/f/...)・Neo4j の volume 名は元 PC のもの(dump は /data マウントから volume を取る)・DB は 10-13 dump とノード数一致・RTX 4070 Ti SUPER・古い環境の写し F:\wsl-export\Ubuntu-20261006-backup.vhdx 315 GB(数日後に消す)。

**Why:** 元 PC 復活で環境を戻す。眠っていた PC の予定タスクが古い内容で上書きする事故を防ぐ。
**How to apply:** 移行は利用者の合図で §2→§3。切替は out --run → in --claim。[[local-backup-setup]] [[env-replacement-pc-20261009]]
- `bash script.sh` はスラッシュ無しだと PATH から探され `$0` も裸の名になる → `readlink -f "$0"` が cwd 相対で壊れる(command -v で先に解く)。複合コマンドの cd は効かないことがあるので絶対パスで呼ぶ。
- 2026-10-13 実施: docker 側ディストロ Ubuntu は wsl セッションが無いと止まる(dump 前に `wsl -d Ubuntu -- sleep` で起こす・out.sh に組み込み済み)。kb_backup のログ名は分単位で衝突していた(pid を足した)。
- 再起動後に USB SSD が未マウントだと /mnt/f が空ディレクトリのまま → kb_backup.sh に未マウントなら書かない門(rc 3)・/etc/fstab に F: を nofail で追加(2026-10-13)。
