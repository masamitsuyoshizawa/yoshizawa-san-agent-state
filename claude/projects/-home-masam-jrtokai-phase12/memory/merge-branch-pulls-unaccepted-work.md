---
name: merge-branch-pulls-unaccepted-work
description: 連絡文を取り込むための git merge <branch> が、相手の受入前の実装まで main へ入れた(2026-10-16 決263-B 追補 3)
metadata:
  type: feedback
---

coord が dkb の req を読むために `git merge dkb` を行い、dkb が「受入が通ってから main へ」と明記していた実装コミット(0b6854dc)まで main に入れて push した。

**Why:** 他セッションのブランチには連絡文以外(受入前の実装)が載っていることがある。ブランチ単位の merge は相手の公開判断を奪う。
**How to apply:** 連絡文だけを取り込むときは `git checkout <branch> -- docs/comms/<file>`(または `git show <branch>:<path>`)で当該ファイルだけを取る。ブランチを merge するのは、相手が「main へ入れてよい」と言った区切りだけ。merge の前に `git log main..<branch> --stat` で連絡文以外の変更が無いことを見る。関連 [[merge-failure-masked-by-pipe]]。
