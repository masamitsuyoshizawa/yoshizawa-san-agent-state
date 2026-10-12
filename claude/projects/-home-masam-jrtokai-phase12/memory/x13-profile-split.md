---
name: x13-profile-split
description: 監査 X13(静的検査)は決229(2026-10-10)で範囲を所有者で分けた — profile c は C の道具だけ・profile all は coord が週 1 回(x13_weekly.sh・日曜 03:30)・結果は knowledge_kb_v8/data/audit/x13_all/
metadata:
  type: project
---

決229(2026-10-10): X13 の profile c は名前こそ C だが走査範囲が全体だったため、他セッションが道具を足すたびに C の監査が FAIL していた(77 → 101 件)。範囲を所有者で分け、profile c = C の道具だけ(PASS)、全体は profile all(coord が週 1 回)。許容表の未使用 6 項目は削除・判定は main の固定版で回し sha16 を記録・許容した項目の件数が増えたら知らせる(--baseline)。

- 道具 = `scripts/x13_static_check.py`(共有・窓口 coord・dkb と ckb の合意で変更)・`scripts/x13_allowlist.json`。
- coord の定例 = `scripts/backup/x13_weekly.sh` → Windows タスク `x13_profile_all_weekly`(日曜 03:30)。記録 = `knowledge_kb_v8/data/audit/x13_all/<日時>_<commit>.json/.log`(git 外)。月曜に所有者別の件数を info。
- 2026-10-10 初回: profile all 未許容 96(bkb 74・dkb 21・fed 1)・各所有者の許容/直すは ckb が取りまとめ。coord の 5 件(kb_backup.sh)は退避の道具として許容(決221)。
