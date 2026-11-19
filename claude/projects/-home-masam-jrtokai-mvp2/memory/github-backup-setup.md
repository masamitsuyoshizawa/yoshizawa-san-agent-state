---
name: github-backup-setup
description: GitHub の非公開リポジトリへの保全(決74・2026-11-11)。push は coord の道具とタイマーだけが行う・各セッションは自分で push しない・data/ と会話の記録は置かない
metadata:
  node_type: memory
  type: project
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-11T10:25:57.333Z
---

2026-11-11、利用者の指示で、本リポジトリを GitHub の非公開リポジトリへ保全する形にした(決74・`docs/comms/20261111-1921_coord_to_acc_dgr_dcr_kgb_dec_github-private-backup.md`)。

- 本体: origin = 非公開の `jrtokai-mvp2`(持ち主は diagram-dynfw-phase0 と同じ個人のアカウント)。main と coord・dgr・dcr・kgb・acc・rev・agy を push する。
- セッションの状況: 非公開の `jrtokai-mvp2-session-backup`。手元の clone は `~/jrtokai-mvp2-session-backup`。台帳・メモリ・作業ツリーの状態・Claude Code の設定の許可したキーだけを写す。
- 道具: `scripts/github_backup.py`(`~/jrtokai-phase12/venv/bin/python` で回す・`--push-project`・`--dry-run`・`--self-test`)。記録は `~/.local/state/jrtokai-mvp2-backup/backup.log`。
- 自動: systemd のユーザータイマー `jrtokai-mvp2-backup.timer`(毎日 21:10・逃した回は次の起動で 1 回)。
- 点検: 本体はブランチごとに origin から足された行を漏れの検査と秘密の型に照らし、当たればそのブランチを push しない。メモリの漏れの当たりは行を伏せ字にして写す。秘密の型は 1 件で全部止める。

**Why:** すべてが PC の WSL の上にだけあり、ディスクの故障で全部を失う状態だった。

**How to apply:** push 済みのコミットは消せない。force push と履歴の書き換えをしない。各セッションは自分で push しない。漏れの検査は、これまでどおりコミットの前に通す。`data/`(確認の答えを含む)と会話の記録は GitHub に無いので、保全されていないと答える。タイマーの push が当たりで止まったら、記録を見て行を直す。関連: [[leak-check-banner-false-positive]]

**追記(2026-11-14):** 11/13 と 11/14 の 2 夜、本体の push が全ブランチで止まった。原因はコミットの文の 1 行(「regulation の受け方」が 8 字の一覧に当たる誤検出)。履歴は書き換えないので、`data/private/leak_allow_8.txt` に 8 字を足して `systemctl --user start jrtokai-mvp2-backup.service` で送り直した。取り込みのたびに `backup.log` の「本体の push」を見る、という決めごとを 1 日抜かした。agy のブランチは 93 件の当たりで送れていない(origin/agy が古く、差が大きい・未解決)。

**追記(2026-11-16・決127):** `github_backup.py` に `on_remote` を足した。ブランチの先が origin のどれかのブランチに含まれる時は、点検せずに早送りで送る。agy のブランチはこれで送れた。同じ日、dcr のコミットの文の一般の言い回しで main ほかが止まり、除外の一覧に足して送り直した。コミットの文の誤検出は繰り返すので、取り込みのたびに backup.log を見る。
