---
name: worktree-data-symlink-gap
description: dkb 作業ツリーの knowledge_kb_v8/data に dkb の参照が無く、照合表が 380 組に縮んで黙って数を誤った(2026-10-14)
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-14T05:21:50.382Z
---

dkb の作業ツリーの `knowledge_kb_v8/data` は実ディレクトリで、`dict`・`eval` だけを本体へ symlink していた。`dkb` の参照が無かったため、作業ツリーで直に読んだ `knowledge_kb_v8/scripts/federation.py` の照合表は 380 組(本来 1,200)、意味照合の語は 0(本来 997)になり、例外も出なかった。覚え書き §12-5 の起点の数(207→221。正しくは 210→224)を誤って報告した。2026-10-14 に `data/dkb` の symlink を足して直した。fed の作業ツリーは data 全体が symlink なので影響は無かった。

**Why:** git 外のファイルが無いと、federation は空の集合へ黙って縮む。数が小さくなるだけで、エラーにならない。

**How to apply:** 作業ツリーで git 外データを使う計測は、先に件数の指紋(照合表 1,200 組・語 997 など)を本体ツリーと突き合わせてから行う。federation は 2 つある(`kg_api/kb/scripts` は診断用・`knowledge_kb_v8/scripts` は explore 用)。組の集合は同じで、並びだけが違う。[[silent-path-drift-masks-cause]] [[audit-run-in-main-tree]]

**再発(2026-10-15・T1 の写し)**: 写しを取るために `git worktree add --detach` で固定した作業ツリーにも、`kg_api/kb/data`(dkb のデータへの symlink)が無かった。診断の意味照合が黙って失敗し(`sources_down` に `意味照合:症状:failed`)、変更の前後の写しがどちらも 1,147/1,345 行で同じ壊れ方をしたので、比較は「同一」と出た。`snapshot_verify` の健康の検査は、この印を見ていない。**How to apply(追補):** 写し用の作業ツリーを作ったら、`knowledge_kb_v8/data` と `kg_api/kb/data` の両方を symlink し、照合表 1,200 組・意味照合の語 997・数問で意味照合の失敗 0、を確かめてから回す。写しの照合では、意味照合の失敗の行を数えて 0 を合格の条件に入れる。[[snapshot-compare-needs-health-and-plan]] [[gpu-contention-silent-semantic-failure]]

追記(2026-10-19・fed 決270): `git worktree add` で新しく作った作業ツリーには、git の外のデータへの記号リンク(`knowledge_kb_v8/data`・`kg_api/kb/data`・`kg_api/kb/index`)が無い。意味照合(`federation.py` の `_symptom_emb.npy` 等)が黙って失敗し、診断の `sources_down` に「意味照合:症状:failed」が出て 22 問中 21 問の中身が変わった(GPU の取り合いと誤認しかけた)。新しい作業ツリーを作ったら、まず既存の作業ツリーの記号リンクを `find -type l` で写し、比べる前に両側の `sources_down` が空であることを照らす。

追記(2026-10-27・bkb 段 3 の受入・4 回目): `git archive` で作った固定の写しに LINKS(sim2b_acceptance の 5 か所)を張り忘れ、さらに `run_tree_s1_acceptance.base_env()` の `KG_VOCAB_REGISTRY` が**作業ツリーの側**(git 外なので無い)を指していた。新・前の両方に「語彙:deferred」「意味照合:症状:failed」が出て、一部の比較は「同じ」と出た。A0(各応答の sources_down が C 以外 0)を全照合の前提に入れていたので落とせた。**How to apply(追補 2):** 新しい受入の道具は、写しを作る関数の中で LINKS を張って `resolve()` を assert し、子の環境で登録簿を本体ツリーへ向けて `is_file()` を assert する。回す前に 3 問で sources_down が null であることを確かめる。

追記(2026-10-31・dkb 決343・5 回目): 演習の台の元にする写しの作業ツリー(`git worktree add --detach`)に、`knowledge_kb_v8/data` だけを張って台を立てたら、回帰の写しが 2 度落ちた。1 度目は問の集合が無く FileNotFoundError(`snapshot_failed`)、2 度目は `kg_api/kb/index` などが無く 646 行中 569 行で意味照合が失敗(`before_sources_down`)。worker のジョブは G3_BUNDLE で `REGRESSION_UNHEALTHY` になる。演習の台(rehearse_env)は `kg_api`・`knowledge_kb_v8/scripts` を元のツリーへリンクするので、元に足りない物はそのまま台に効く。**台の元に要るリンクは 5 本**: `knowledge_kb_v8/data`・`knowledge_kb_v8/index`・`kg_api/data`・`kg_api/kb/data`・`kg_api/kb/index`(どれも本体ツリーへ)。**How to apply(追補 3):** 写しの作業ツリーを作ったら、前の写しの `find -maxdepth 4 -type l` を写して 5 本をそろえ、台を立てる前に回帰の写しを 1 回直に回して `sources_down` が 0 であることを確かめる。
