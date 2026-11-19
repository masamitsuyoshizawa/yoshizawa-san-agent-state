---
name: empty-filename-var-widens-git-add
description: 連絡文を書く Python が assert で止まった後、ls|grep で取った名が空のまま grep -c と git add に渡り、ディレクトリ全体に及んだ・名が空なら止めてから進む
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-19T11:20:34.222Z
---

連絡文を Python で書き、続けて `f=$(ls docs/comms | grep …)` → `grep -c 語 "docs/comms/$f"` → `git add docs/comms/INDEX.md "docs/comms/$f"` → commit → push と 1 つの命令でつなぐと、Python の assert(置換子の数など)で書けなかった時に `$f` が空になり、`grep` は docs/comms の全部のファイルを数え(出力 467KB)、`git add` は `docs/comms/` の全部を加える。2026-11-19 20:19 は変更が無くコミットに至らなかったが、ほかのファイルに変更が在れば、関係の無い物がコミットと push に入る。同じ日に、空の `$f` で grep が標準入力を待って止まる形も 1 度起きた。

**Why:** Bash ツールでは set -e が効かない([[bash-tool-set-e-ineffective]])ので、前の段の失敗で後の段が止まらない。置換子の数の assert は、相手の道を本文に何度書いたかを数え違えると外れる([[placeholder-substitution-collisions]])。

**How to apply:** 名を取った直後に `if [ -z "$f" ]; then echo 書けていない; exit 1; fi` を置く。`git add` には、空になり得る変数を組んだ道を渡さない。assert の数は、本文を書いた後に、その字が出る所を数えてから決める。
