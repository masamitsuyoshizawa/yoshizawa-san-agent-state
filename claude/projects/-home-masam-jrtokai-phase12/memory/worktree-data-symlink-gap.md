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
