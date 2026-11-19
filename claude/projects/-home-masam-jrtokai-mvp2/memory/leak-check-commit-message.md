---
name: leak-check-commit-message
description: leak_check の worktree と diff はコミットの文を照らさない・coord の履歴の検査は照らす・コミットの文も shingles で照らしてから(2026-11-12・kgb・c01c9d2 を push の前に amend)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 85f25ae1-06b2-4f77-8b2b-5e740c5782d5
  modified: 2026-11-12T12:36:14.965Z
---

`scripts/leak_check.py` の `worktree` と `diff <ブランチ> --base main` は、ファイルの行だけを照らし、コミットの文は照らさない。2026-11-12 に kgb のコミットの文の 1 行(英字の種類の名の後に「の」が続く言い回し)が 8 字に当たったが、diff の検査は「一致 0」と出た。coord の履歴の全体の検査(決74)は、コミットの文も照らす。

**Why:** push(決74・毎日 21:10)の後は、コミットの文を直せない(force push と履歴の書き換えをしない)。push の前なら、手元のコミットを amend で直せる。

**How to apply:**
- コミットの後に、`git log -1 --format=%B | python -I -c "...lc.shingles(l) & lc.load_lists()['sh']..."` で、コミットの文の当たりの行の数を出す(字は出さない)。
- 当たれば、`git merge-base --is-ancestor <commit> origin/<ブランチ>` で push 済みかを確かめる。push の前なら amend で文を直し、coord に直したことを知らせる。push 済みなら直さず、coord に知らせる。
- `load_lists()` は 1 回の読み込みが重い(8 字の一覧が 100 万を超える)。コミットごとに起動し直さず、1 つの過程でまとめて照らす(シェルの for で回すと 2 分を超えた)。

関係: [[leak-check-banner-false-positive]] [[leak-check-diff-zero-lines]] [[github-backup-setup]]

**追補(2026-11-16・dcr):** 9fcba3b の文を照らさずにコミットし、GitHub への保全の送りが止まった(coord が除外の一覧に足して送り直した)。コミットの文は先にファイルへ書き、`leak_check.py files <文のファイル>` を通してから `git commit -F <文のファイル>` でコミットする。
