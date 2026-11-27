---
name: branch-only-file-removed-on-main-merge
description: coord が本体から外した自分の枝のファイルは、main を枝に取り込むと枝からも消える・--no-commit で止めて戻してからコミットし、本体へ戻る事を coord に知らせる
metadata:
  type: feedback
---

coord が枝 bkb を本体へ取り込む時に、ファイル 1 つ(例: 2026-11-24 の `knowledge_kb_v8/scripts/bkb/r365_mask_scope_count_bkb.py`)を「本体にはまだ入れない」として、本体の側で削除のコミット(45114a7e8)を足した。そのまま `git merge origin/main` を枝に打つと、削除が流れてきて**枝からも消える**(合併は自動で成功し、rc 0)。

**Why:** 枝の側はそのファイルを取り込みの基点の後に変えていないので、本体の削除がそのまま採られる。coord の頼みは「枝からは消さない」だった。

**How to apply:** 本体から外されたファイルが枝に在る時は、`git merge --no-commit --no-ff origin/main` で止め、`git status` でそのファイルが `D` になっていないかを見て、`git checkout HEAD -- <道>` で戻してからコミットする。戻した後は、枝を本体へ取り込むとファイルが本体に戻る形になるので、coord に連絡箱で知らせる(外し直しが要る)。普段の連絡の前の `git merge origin/main` でも、この形が残っている間は毎回確かめる。関連: [[merge-failure-masked-by-pipe]]・[[coord-merges-peer-branch-when-peer-main-push-blocked]]

**再発(11-28 に見つけた)**: 11-24 18:17 の取り込み 897c83261 で、枝に留める台本 2 つ(r365_mask_scope_count_bkb.py・r364_bundle_form_examples_bkb.py)が消えていた。毎回の取り込みの後に「台本は在る」を test -f で見るつもりが、その行を省いた取り込みが在った。**取り込みの命令の後に、枝に留める物の在り無しを毎回照らす行を必ず付ける**(52e32b957 で戻した)。
