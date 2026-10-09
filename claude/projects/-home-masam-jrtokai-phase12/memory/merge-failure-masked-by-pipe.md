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
