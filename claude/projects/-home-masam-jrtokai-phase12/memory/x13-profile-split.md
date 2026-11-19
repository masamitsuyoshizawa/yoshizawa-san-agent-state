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
- 決237(2026-10-13 20:1x): profile c の範囲から kg_api/ の C 以外を外す(決229 A の穴・fed の道具 2 本が C に落ちて FAIL した)+ bkb の許容 35 項目(74 件)を取り込む。ckb 実装・dkb 合意・LOCKS。fed の道具は fed が是正済み(rep 2004・移行前後の M1 は丸め 50/50・bit 10/50)。
- 決237 実装完了(ckb 2026-10-13 20:12・ed25a83c): profile c 449/未許容 0・all 未許容 21(dkb のみ)・G0 相当 190/0・許容表 117 項目・audit_c PASS 8。dkb の部品 5 本はファイル単位で dkb に数える。

追記(2026-10-16・決263-B の演習で発見): **B/D-KG の投入の G0 は、`knowledge_kb_v8/scripts/dkg` などの `.py`(test_ 以外)を全部 X13 で照らす。** dkb が 10-15 00:18 に入れた集計の台本 `n2_stage0_breakdown.py` の閉じた口の URI(`bolt://127.0.0.1:1`)が未許容で、それから本番の投入の G0 が止まる状態だった。気づいたのは 1 日半後の演習。**dkg/ などの G0 の走査の範囲に台本を足したら、その場で X13 の未許容を数える**(`scripts/x13_static_check.py` と `x13_allowlist.json`・本体ツリーで)。許容は理由と dec 番号を書いて足し、ckb と coord に info。

## 統合した記憶: x13-bkb-scope-split(2026-10-19 に索引の行数を減らすため統合)

決229(2026-10-10): X13 の範囲を所有者で分け、bkb 分は bkb が許容/直す/消すを決める。rep 20261010-1209(bkb → ckb)。

- 直した: env_default_uri 2(canonical_fix_acceptance・mask_field_check)= KB_ACCIDENT_NEO4J_URI 未設定なら止める(今後この 2 本を回すときは URI を明示する)。
- 許容 70 件・33 ファイル(案 = knowledge_kb_v8/scripts/bkb/x13_allow_proposal_bkb.json・ckb が x13_allowlist.json へ取り込む): A 判定の禁止語の一覧・C 一時の口(127.0.0.1 中継・一時コンテナ)と届かない口の否定例・D 正本の読み取りの受入の台。
- bkb の道具に正本への書き込みは無い(書き込みの語は全部「書き込みを拒む門」の正規表現)。
- 消さない理由: rep が道具をパスと sha16 で引く。

**Why:** 許容表はファイル単位なので、許容したファイルに後から URI を足しても通る。
**How to apply:** 新しい bkb の道具は正本の URI を run_tree_s1_acceptance.base_env から取り、`bolt://` や正本のポートの文字を新しいファイルに散らさない。X13 の変異の確かめは許容していないファイルに差し込む。関連 [[audit-run-in-main-tree]]

追記(2026-11-09・dkb): **X13 はディレクトリの走査で `test_` で始まるファイルを外す作り**(`list_files`)。`--paths` に試験のファイルを並べると、使い捨ての DB の URI や DELETE が拾われる。そこで許容の表に試験のファイルを足すと、全体の照らし(`--check-allow`)では「使われていない許容」になり表の健康を壊す。**コミット前の X13 は試験でない道具に当てる**(決352 段 2 で足して戻した・push の前に手元で戻した)。
