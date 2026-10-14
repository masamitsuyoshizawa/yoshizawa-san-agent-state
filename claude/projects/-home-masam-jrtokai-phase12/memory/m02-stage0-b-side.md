---
name: m02-stage0-b-side
description: 決246 M02(異原因の対照 CBR)段 0 の B 側(bkb rep 20261014-2228)— 両方 21/50 だが 2 軸の類似は選別にならず 3 軸で 0・既存 b-3a と同じではない
metadata:
  type: project
---

2026-10-14 bkb が決246 (2)(a)(b) を回答(rep `20261014-2228_bkb_to_dkb_rep_m02-stage0-b-side.md`・道具 `m02_stage0_b_side.py` 3e5f28d77fd19dc5・コミット d8f1bfff・出力 git 外 `knowledge_kb_v8/data/eval/m02_stage0_20261014/bkb/`)。

- (a) 開発 50 問・facets の一致軸 2 以上・当該と双子を除く: 同原因の支持と異原因の対照が両方ある問い 21/50(確定 19)。5 問以上は電子踏切制御装置系 11/17 だけ。ただし類似は「設備クラス + 現象」の 2 軸がほぼ全部(延べ 4,860/4,888)で、同系統は類似の中央値 242 件・対照 189 件 = 選別になっていない。3 軸以上では両方 0。番号の要る切り口は第一報に番号が無く 50 問とも照会されない。
- (b) b-3a(`accident_diag_engine` c9e8979dccef191f)は同原因の支持(support_acc_ids)と、候補を分ける特徴 1 つの問いを既に持つ。候補ごとの同・異の対の提示・観測差の明示・役割は無い → 既存と同じではない。案 A/D は既定 OFF。federate の再ランクは A の文書。「facets の 4 軸ボーナス」は実装を見つけられず coord に確認を依頼した。
- 判定(d)は dkb。dkb 用 jsonl は m02_b_similar_strict.jsonl(9b8c803fb84f696d)。

**Why:** M01・M03 が保留になり、M02 も保留なら調査を閉じる分岐(決246 (2)(d))。数の上では (a) の 5 問を満たすが、中身は弱い。
**How to apply:** M02 の判定や再開の話が来たら、まず「2 軸の類似は選別にならない・3 軸で 0」を前提に読む。台の注意: `run_tree_s1_acceptance.base_env()` の `KG_VOCAB_REGISTRY` は作業ツリーを指すので、作業ツリーから facets を回すと 503(vocabulary_db_deferred / registry_missing)になる。本体ツリーのパスに上書きし、200 以外で止める。[[worktree-data-symlink-gap]] [[explore-proto-item]] [[snapshot-compare-needs-health-and-plan]]
