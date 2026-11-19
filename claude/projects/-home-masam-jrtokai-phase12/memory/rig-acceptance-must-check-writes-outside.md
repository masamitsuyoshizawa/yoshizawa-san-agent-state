---
name: rig-acceptance-must-check-writes-outside
description: 台の受入が正本の digest だけを照らし、工程が台の外(D: の退避・本体の実データ)へ書くのを見逃した(決343 段 2 の G10・2026-11-05 に bkb の指摘で発覚)
metadata:
  type: feedback
---

2026-10-31 の決343 段 2(台の commit 版)で、工程の G10 の退避 `backup_llm_artifacts.py` が、読む元を本体ツリーに、書き先を `/mnt/d/jrtokai-phase12-backup` に決め打ちしていた。そのため、台から呼ばれても、本体の実データ(個人情報の印のあるアーカイブ 4 つを含む・暗号化なし)の世代を D: に作った。dkb の受入は正本の digest と本体の評価資産だけを照らしていて、5 日後に bkb の指摘で分かった(info 20261105-1158・利用者の判断待ち)。似た型に、演習のツリーの G8 の辞書の追加が、記号リンクを通って道具の在るツリー(git 管理)へ書く件がある。

**Why:** 「正本が変わらない」は「台の外に何も書かない」の一部でしかない。工程の段には、決め打ちの道や記号リンクを通って台の外へ書くものがある。守る対象の一覧(正本)から照らすと、それ以外の書き先が見えない。

**How to apply:** 台・演習・写しで工程を回す受入では、前後で次を比べる。
- 本体ツリーと起動元の作業ツリーの `git status --porcelain`
- git 外のデータの置き場の変化
- 退避の置き場(D:)の世代の一覧
- `git worktree list`

あわせて、工程の各段の道具に、決め打ちの道(`MAIN`・`/mnt/d`)が無いかを grep する。台の元は、専用の作業ツリーにする(`rehearse_env.py` の `LINKS` は道具の在るツリーを指す)。関連: [[count-side-effects-in-judgement]] [[gate-allowlist-not-denylist]] [[intake-accident-sat-registration-item]]

追記(2026-11-05 12:09): 「git の木か」を記号リンクの下の道で `git -C` に問うと、リンクの先の実装のツリーが答えて rc 0 になる。台の一時の木かを判じるには、リンクを解かない木の根(道具の `REPO` = abspath から上の親)で問う(rc 128)。使い捨ての置き場で確かめた。`DKB_MAIN_TREE` は台が実装のツリーを渡すので、本体かどうかの目印にならない。正本かどうかを `kb_conn.CANONICAL` で判じると、EC2 のトンネルの口(19890・19891)は外れる。
工程 `ingest_pipeline.py` の `HERE` は `.resolve()` で解く。それでも台の木に留まるのは、`make_tree` が `accident_kb_v7/scripts` を copytree で写すため(リンクにしたら照らしが黙って真になる)。処理役 `knowledge_kb_v8/scripts/bkb/intake/runner.py` も工程を `.resolve()` した道で起動する(95 行目)ので、工程の側を abspath で照らしても、scripts がリンクなら効かない。守りは「scripts を写す」ことの照らしと試験を対にする(bkb)。退避の道具は `run_py` が解いていない V8 の道で起動するので、`__file__` は台の木のまま。REPO は `os.path.abspath` で作り、`.resolve()`・`realpath` は使わない(bkb の注意 2026-11-05)。
