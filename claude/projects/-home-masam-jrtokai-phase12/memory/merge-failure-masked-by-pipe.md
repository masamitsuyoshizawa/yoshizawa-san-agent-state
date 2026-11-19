---
name: merge-failure-masked-by-pipe
description: "`git merge ... | tail -1` は失敗の終了コードを隠す。その後の `git add docs/comms/ && git commit` が競合マーカーごとコミットし main へ流れる(2026-09-24 に coord が実演)"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-24T03:36:31.925Z
---

2026-09-24 12:37、coord が bkb ブランチを `git merge -q --no-edit bkb 2>&1 | tail -1` で取り込み、競合(dec 1233 の status 行・fed と bkb の注記)が出たのに、パイプで終了コードが消えたため次の `git add docs/comms/ && git commit` がマーカーごとコミット(9c9d07f9)し、`--ff-only` で main にも流れた。grep のマーカー検査はコミットの前に置いていたが、検出しても止まらない書き方だった(出力を見るだけ)。279f37a3 で両注記を残して解消。

**Why:** 連絡箱の status 行は複数人が同時に注記を足すので競合が日常的に起きる。マーカーが main に入ると索引は通るのに status が 2 本ある状態が全ブランチへ伝播する(2026-09-19 に dkb が 5 件発見した型と同じ)。

**How to apply:** merge は `git merge … || { echo CONFLICT; exit 1; }` のように失敗で止める。コミットの前のマーカー検査は `! grep -rl '^<<<<<<<' docs/comms` のように**検出したら止まる**形にする。`comms_resolve_status.py` は「status 行 2 本 + マーカー 3 行」の形を解消できない場合があるので、解消後に `grep -c '^<<<<<<<'` と `t.count("\nstatus: ")==1` で確かめる。[[multi-recipient-comms-stay-open]] [[criterion-not-tool-output]]

**2026-09-27(dkb・2 回目)**: `python3 make.py | grep … | tail -1 || exit 1` で一時コンテナの起動失敗(パスワード先頭「-」)が隠れ、続く「戻した D-KG」の測定が正本で回って差 0 に見えた。記録の頭の宛先(dkg_uri)で気づいた。**長い前処理 → 測定の連鎖は `set -o pipefail` と、前処理の成果物(接続情報ファイル)の有無の確認を必ず挟む。記録の頭に宛先を残し、比べる前に宛先が違うことを確かめる。**

**2026-09-28(fed・3 回目)**: LOCKS.md の競合を Python で解消し、復元の照合(assert)で止まって書き戻さなかったのに、次の `grep -c …; python3 comms_index.py; git add … && git commit … && git merge --ff-only` を `;` でつないでいたため、マーカーごと ff474add が main へ入った(041da522 で直した)。**パイプだけでなく `;` でも失敗は消える。解消から ff までは `set -e` にし、マーカー 0 を `test` で確かめてから add する。** さらにその事故の知らせを、変数を埋め込むために未クォートの heredoc で書き、本文のバッククォート(`git add` など)が実行されて消えた(CLAUDE.md の禁止事項。変数は Python の置換で入れる)。


**同型の再発(2026-09-28 fed・ff474add)**: 競合解消の道具が照合で止まって書き戻さなかったのに、次の命令を `;` でつないでいたため add・commit・ff が走り、LOCKS.md の競合マーカーが main に入った(041da522 で是正)。パイプだけでなく `;` も失敗を隠す。連鎖は `&&` で、検査は検出したら止まる形に。同じ便で未クォート heredoc のバッククォート消失も再発(Python で直した)。


**coord 自身の再発(2026-09-28 a2d0aa1d)**: `! grep -rl '^<<<<<<<' … && echo no-markers; python3 …; git add … && git commit …` の形は、grep が見つけても `;` の後の add/commit/ff が走る。fed の事故(同日 ff474add)の直後に同じ型を踏んだ。**規律: 連鎖は 1 つの `&&` 列にする(検出したら後段が走らない)。`echo no-markers` のような確認の分岐を `;` で切らない。** 是正: 次のコミットで両注記を残して解消し main で 0 件を確認。

**再発(2026-09-28 fed)**: 未コミットの変更があるまま `git merge main 2>&1 | tail -1` → 「Updating …」だけ見えて実際は中止(ローカル変更が上書きされるため)。先に自分の変更をコミットしてから merge し、出力はファイルへ落として rc を見る。また、競合マーカーの検査を `git grep '^<<<<<<< ' docs/comms` にすると、マーカーを本文に引用した過去の連絡(20260919-0452)に当たって止まる → 検査は今回 staged のファイルに限る。

## 追記(2026-10-09)
- **scratchpad の道具は新しいセッションで消える**。`git merge X || (python3 $SP/resolve.py && git add -u && git commit)` の形で解消スクリプトが無いと、`||` の後の `git add -u` が **競合マーカーごとステージして commit し push まで通る**(10/09 に 2 件・数分間 main に載った)。守り = 解消スクリプトはリポジトリ内(scripts/)に置くか、merge の後に `git status --short | grep ^UU` と `grep -l '^<<<<<<< ' docs/comms/*.md` を **commit の前に** 止まる形で入れる。
- 連絡文の本文に例としてマーカーを引用した便(20260919-0452)があるので、全体 grep の 0 件を期待せず、対象を merge で触った便に限る。

**2026-10-15 にも同型(ckb)**: 競合を解消する Python が assert で止まったのに、**同じ Bash 呼び出しの次の行の `git add … && git commit --no-edit` が走り、競合マーカーごと merge コミットを作った**(push 前に気づき amend で直した)。原因 = 検査と後段を改行で並べると、前段の失敗で後段が止まらない。**How to apply: 解消のスクリプトとコミットは別の呼び出しに分けるか、`python3 … && git add … && git commit …` と && でつなぐ。コミットの前に `git grep -n "^<<<<<<<"` で 0 件を確かめる。**

**連絡を早送りの成否より先に送った(2026-10-16・dkb・決260)**: コミット・早送り・push・SendMessage を 1 回の応答で並べて呼んだため、`merge --ff-only` が失敗(ff_rc=128・main が先に進んでいた)したのに、coord と bkb へ「main に入れた」と送ってしまった。取り込み直して早送りし、訂正を送った。**How to apply:** 連絡(SendMessage・rep の「main に入れた」)は、早送りと push の終了コードを確かめた**後の**応答で送る。同じ応答の中で並べて呼ばない。

## 統合した記憶: merge-branch-pulls-unaccepted-work(2026-10-19 に索引の行数を減らすため統合)

coord が dkb の req を読むために `git merge dkb` を行い、dkb が「受入が通ってから main へ」と明記していた実装コミット(0b6854dc)まで main に入れて push した。

**Why:** 他セッションのブランチには連絡文以外(受入前の実装)が載っていることがある。ブランチ単位の merge は相手の公開判断を奪う。
**How to apply:** 連絡文だけを取り込むときは `git checkout <branch> -- docs/comms/<file>`(または `git show <branch>:<path>`)で当該ファイルだけを取る。ブランチを merge するのは、相手が「main へ入れてよい」と言った区切りだけ。merge の前に `git log main..<branch> --stat` で連絡文以外の変更が無いことを見る。関連 [[merge-failure-masked-by-pipe]]。

**追記(2026-10-17)**: rev(外部エージェント・ブランチは連絡文と docs だけ)については `git checkout rev -- docs/comms/` で取り込むと、rev 側が後で main を merge したときに「両側で追加」の競合が起き、解消時に rev の status 注記を落とした(2 回)。rev は `git merge rev` で取り込む方が安全(実装は載らない)。競合を `--theirs`/`checkout main --` で解く前に、`comms_resolve_status_lines.py` が合成した結果を上書きしない。解いた後は両側の status 行を比べて注記が落ちていないか確かめる。

**再発(2026-10-30 11:1x・coord)**: rev の代行コミットの取り込みで、`set -e` を途中で `set +e` に戻した後の `git merge rev` が status 行 6 件で競合して止まったのに、続く `comms_index.py`・`git add -u docs/comms`・`git commit`・`merge --ff-only`・`push` がそのまま走り、**競合マーカーごと main(5b228c046)に入れて push した**。10 分以内に両方の注記を残して解消(aedfe4ab0)・他の枝は取り込んでいなかった。**How to apply(追加)**: merge の後は必ず `git diff --name-only --diff-filter=U` が空であることを assert してから add/commit する。`set +e` に戻した区間に merge を置かない。rev の取り込みは「main を rev に先に取り込ませてから」行う(rev の基点が古いほど status 行が競合する)。

追記(2026-11-05・dkb): 同じ型で、コミットの前の X13 を `... | tail -1` で見て、`X13: FAIL` の行を出していたのにコミットと push まで進めた(&& の連鎖はパイプの末尾の tail の終了値しか見ない)。門になる点検(X13・試験)は、出力をファイルへ落としてから終了値を変数に取り、`if [ $rc -eq 0 ]` の中でだけコミットする。

追記(2026-11-10・bkb): 変異の回し(`mut_gen.py … | tail -3`)の後に `; cd … && git add … && git commit -m "…変異 2/2 が判定で落ちる…" && ff && push` を同じ呼び出しで並べた。変異の 1 つが試験の時間の上限で打ち切られ(判定を出さずに例外)、それでもコミットと main への早送りまで進み、**確かめていない「変異 2/2」をコミットの文に書いて main へ入れた**(667d22dc1)。**How to apply:** 変異・試験の回しと、その結果を主張するコミットは別の呼び出しに分ける。コミットの文の数は、結果を読んだ後で書く。

**追記(2026-11-10・coord)**: 本体ツリーへの早送りの前は、`origin/main` でなく手元の `main` を取り込む。他のセッションは手元の main へ早送りしてから push するので、`origin/main` を取り込むと手元の main より遅れ、`merge --ff-only` が「Diverging」で何度でも落ちる(main が動いたように見える・3 回繰り返して気づいた)。ついでに版の照らしも古い木で測ってしまう(受入の設計の sha が古い値になった)。

**追記 2(2026-11-11・coord)**: `G=$(grep -c ...) && ...` の連鎖は、一致が 0 件のとき grep の終了値 1 で後ろが黙って走らない(数えた値は 0 で正しいのに、早送りが起きない)。台の予約が無いときほど止まる。数を取る代入は `|| true` を付けるか、`if` の中で比べる。
