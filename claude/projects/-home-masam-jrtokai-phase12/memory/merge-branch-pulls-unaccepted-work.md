---
name: merge-branch-pulls-unaccepted-work
description: 連絡文を取り込むための git merge <branch> が、相手の受入前の実装まで main へ入れた(2026-10-16 決263-B 追補 3)
metadata:
  type: feedback
---

coord が dkb の req を読むために `git merge dkb` を行い、dkb が「受入が通ってから main へ」と明記していた実装コミット(0b6854dc)まで main に入れて push した。

**Why:** 他セッションのブランチには連絡文以外(受入前の実装)が載っていることがある。ブランチ単位の merge は相手の公開判断を奪う。
**How to apply:** 連絡文だけを取り込むときは `git checkout <branch> -- docs/comms/<file>`(または `git show <branch>:<path>`)で当該ファイルだけを取る。ブランチを merge するのは、相手が「main へ入れてよい」と言った区切りだけ。merge の前に `git log main..<branch> --stat` で連絡文以外の変更が無いことを見る。関連 [[merge-failure-masked-by-pipe]]。

**追記(2026-10-17)**: rev(外部エージェント・ブランチは連絡文と docs だけ)については `git checkout rev -- docs/comms/` で取り込むと、rev 側が後で main を merge したときに「両側で追加」の競合が起き、解消時に rev の status 注記を落とした(2 回)。rev は `git merge rev` で取り込む方が安全(実装は載らない)。競合を `--theirs`/`checkout main --` で解く前に、`comms_resolve_status_lines.py` が合成した結果を上書きしない。解いた後は両側の status 行を比べて注記が落ちていないか確かめる。
