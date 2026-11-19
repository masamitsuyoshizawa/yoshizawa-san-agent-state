---
name: merge-alone-before-closing-steps
description: 取り込みの git merge は単独の呼び出しで回し、競合を見てから閉じる注記とコミットへ進む(2026-11-01・coord・6dd946c)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-01T15:16:28.210Z
---

`git merge` を、閉じる注記の Python と && でつなぎ、索引の再生成とコミットを ; でつないだ。INDEX.md が競合すると merge が止まり、&& の先の閉じる注記は飛ばされた。それでも ; の先は走り、競合を残したままの取り込みが「info を閉じる」という名のコミットになって、main まで進んだ(6dd946c・後の 87a0ddf で閉じ直した)。

**Why:** && の連なりは失敗で止まるが、; の後は走る。merge の途中(MERGE_HEAD あり)の `git commit` は、黙って取り込みのコミットになる。
**How to apply:** coord の取り込みの手順では、merge を 1 回の呼び出しにして終了の値と `git status` を見る。閉じる注記・索引・漏れの検査・コミットは、その後の別の呼び出しで行う。[[pipefail-before-chaining-checks]] と同じく、確かめを一続きにしない。

**追補(2026-11-02・kgb):** 頭ではこの規則を知っていたのに、LOCKS の予約の Python とコミットを、`git merge main &&` の後ろにつないで回した。status の行の競合で merge が止まり、&& の先は走らなかったので害は無かった。急いで手順をまとめるときほど、merge だけを先に 1 回で回す。

**追補(2026-11-01・決36):** 競合を解いた後、MERGE_HEAD が残ったまま dec を書いて一緒にコミットした。その結果、件名が「comms: 決36…」の取り込みのコミット(親が 2 つ)ができた。main へ進める前に気づき、件名を merge に直した(16f5e22)。競合を解いたら、まず取り込みのコミットを「merge: …」の件名で単独で作る。dec や閉じる注記は、その次のコミットにする。

**追補(2026-11-13・kgb):** `git merge main 2>&1 | tail -1 && python3 …` と、merge の出力を tail に通してから && でつないだ。pipefail が無いので、競合(LOCKS.md)でも && の先が走り、LOCKS の解放の書き換えと `git add`・`git commit` が、競合の印を残したままの取り込みのコミット(e3da77f・親 2 つ)になった。手元のコミットを `git reset --hard` で戻すことは許可されなかったので、印を外す直しのコミット(1223585)で前へ直した。merge は、出力を tail に通さず、1 回の呼び出しで回して終了の値と `git status` を見る。

**追補(2026-11-14・dcr):** merge を単独で回したのに、出力の「Automatic merge failed」を見ないまま、次の呼び出しで連絡文を書いて `git add` した。コミットは競合で断られたが、競合を解いた後の `git commit --no-edit` に、先に add した rep が入った(601c9f6)。merge の出力に failed があれば、ほかのファイルを add する前に、競合を解いて取り込みのコミットを単独で作る。

**追記(2026-11-16・coord):** 競合を解く道具(resolve_status.py)が assert で止まったのに、後ろの検査とコミットが `;` でつながっていて、競合の印の残ったファイルをコミットし main に反映した(決128 の status の行・すぐ直した)。漏れの検査は競合の印を見ない。解く道具と、印の有無の照らし(`! grep -rq '^<<<<<<< ' docs scripts`)を `&&` でつなぎ、通った時だけコミットに進む。status が open と closed に分かれた競合は、道具が扱えないので手で解く。
- 2026-11-17(kgb): 文書のコミットの && の後に git merge main をつないだ(3 回目・Already up to date で害なし)。コミットと取り込みは、別の呼び出しに分ける。

再発(2026-11-18・dcr): `git merge ... | tail -1; git diff --diff-filter=U; python3 - <<連絡文を書く>>` と 1 つの呼び出しにつないだ。取り込みが競合で止まったのに、連絡文を書き、git add まで進んだ(コミットは競合で止まった)。連絡文を退け、競合を解いて取り込みのコミットを単独で作り、連絡文を戻して `re:` も実物の名に直してからコミットした(88c6876 → 0ace38d)。
**How to apply(足し):** 取り込みの呼び出しは、merge と競合の確かめだけで終える。連絡文を書く呼び出しは、競合が 0 と見てから別に出す。

**再発(2026-11-19・dgr・36d7cf4 → feac5ae)**: `git merge --no-edit main 2>&1 | tail -1;` の後に `;` で rep の書き出しとコミットをつないだ。決161 の status の行(dgr の既読と kgb・acc の既読)が競合し、merge は止まったが、`;` の先で `git add` したファイルごと取り込みのコミットになり、競合の印が残った。直しは新しいコミットで 3 つの注記を 1 行にまとめた。
**How to apply(足し)**: dgr も、取り込みは `git merge --no-edit main` を単独の呼び出しで回し、出力の「CONFLICT」か `git diff --name-only --diff-filter=U` を見てから、次の呼び出しに進む。コミットの前に `git grep -l '^<<<<<<< '` が 0 であることも照らす。
