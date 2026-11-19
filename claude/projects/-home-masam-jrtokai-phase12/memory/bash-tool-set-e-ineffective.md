---
name: bash-tool-set-e-ineffective
description: Claude Code の Bash ツールでは set -e が効かない(フラグは立つが失敗の後も次の行へ進む)。止めたい所は || exit 1 か && で書く
metadata:
  type: feedback
---

Bash ツールの中で `set -e; false; echo x` は x を出す(`$-` に e は在る・2026-11-11 実測。コマンドが `||` か `&&` の文脈で評価されている時の bash の仕様と同じ振る舞い。原因の特定まではしていない)。`set -e; python3 - <<'X' ... sys.exit(1) ... X` の後も次の行へ進んだ。`false || exit 1` は止まる(rc 1・次の行は出ない)。

**Why:** 2026-11-11(fed): 連絡箱の書き換えの Python の assert が落ちたのに `set -e` を信じた手順がそのまま進み、「ack を閉じた」と書いたコミットを main へ早送りして push した(実際は閉じる処理は走っていない。ack は dkb が最初から closed で出していたので害は無かった)。

**How to apply:** 複数の手順を 1 回の Bash で続けるとき、`set -e` に頼らない。後続を止めたい手順の直後に `|| exit 1` を置くか、`&&` でつなぐ。Python の heredoc は閉じの行の次に `[ $? -eq 0 ] || exit 1` を置く。commit と早送りと push は、前の手順の成否を見てから別の呼び出しで行うのが確実。[[merge-failure-masked-by-pipe]]

**2026-11-19 の実例**: 競合を解く Python が assert で落ちた後に、同じ命令の中の `git add && git commit --no-edit` が走り、**競合の印ごと取り込みのコミットを作った**(push の前に気づいて amend で直した)。失敗したら先へ進まない形(`RC=$?; if [ "$RC" = "0" ]; then … fi`)で書く。競合を解いた後は、印の数を数えてから commit する。
