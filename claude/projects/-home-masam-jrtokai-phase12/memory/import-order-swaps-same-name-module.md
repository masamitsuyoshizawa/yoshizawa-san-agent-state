---
name: import-order-swaps-same-name-module
description: "run_fed_vs_dkb を先に読むと探索先の先頭に kg_api/kb/scripts が入り、`import federation` が kg_api 側(照合表の並びが違う)に入れ替わる・読み手を最初に固定し __file__ を確かめる(2026-10-14)"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-14T05:42:21.418Z
---

`federation.py` は `knowledge_kb_v8/scripts/` と `kg_api/kb/scripts/` の 2 つがある。照合表の組の集合は同じだが、並びが違う(LOO 647 問のうち 12 問で D の起点の並びが変わる・dkb 調べ)。

`run_fed_vs_dkb` を import すると、`sys.path` の先頭に `kg_api/kb/scripts` が入る。その後の `import federation` は kg_api 側に当たる。explore 改訂の台で、双子の表を確かめるために `run_fed_vs_dkb` を先に読み、照合表を kg_api 側で読んでいた(sha16 b7bd5bea… / 正しくは knowledge_kb_v8 側 e0185306…)。照合の道具が先に `run_fed_vs_dkb` を読む経路でも、同じことが起きた。

**Why:** 同じ名前のモジュールは、最初に読んだ物が `sys.modules` に残る。どれが当たるかは import の順で決まり、黙って入れ替わる。

**How to apply:** 照合表のように並びが結果に効くモジュールは、入口の最初に読み込んで固定する。`__file__` を期待のパスと照らし、違えば止める。記録には `__file__` と中身の sha16 を残す。旧版の置き場から回すときも同じ順にする。[[silent-path-drift-masks-cause]] [[worktree-data-symlink-gap]]
