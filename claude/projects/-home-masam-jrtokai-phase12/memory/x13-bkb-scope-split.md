---
name: x13-bkb-scope-split
description: 監査 X13(静的検査)の bkb 分 74 件の扱い(決229・2026-10-10)— 直す 2・許容 70(33 ファイル・群 A/C/D)・消す 0・許容はファイル単位
metadata:
  type: project
---

決229(2026-10-10): X13 の範囲を所有者で分け、bkb 分は bkb が許容/直す/消すを決める。rep 20261010-1209(bkb → ckb)。

- 直した: env_default_uri 2(canonical_fix_acceptance・mask_field_check)= KB_ACCIDENT_NEO4J_URI 未設定なら止める(今後この 2 本を回すときは URI を明示する)。
- 許容 70 件・33 ファイル(案 = knowledge_kb_v8/scripts/bkb/x13_allow_proposal_bkb.json・ckb が x13_allowlist.json へ取り込む): A 判定の禁止語の一覧・C 一時の口(127.0.0.1 中継・一時コンテナ)と届かない口の否定例・D 正本の読み取りの受入の台。
- bkb の道具に正本への書き込みは無い(書き込みの語は全部「書き込みを拒む門」の正規表現)。
- 消さない理由: rep が道具をパスと sha16 で引く。

**Why:** 許容表はファイル単位なので、許容したファイルに後から URI を足しても通る。
**How to apply:** 新しい bkb の道具は正本の URI を run_tree_s1_acceptance.base_env から取り、`bolt://` や正本のポートの文字を新しいファイルに散らさない。X13 の変異の確かめは許容していないファイルに差し込む。関連 [[audit-run-in-main-tree]]
