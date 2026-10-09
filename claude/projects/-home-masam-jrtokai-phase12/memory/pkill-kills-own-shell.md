---
name: pkill-kills-own-shell
description: `pkill -f '<pattern>'` は、そのパターンを含む自分のシェル(Bash ツールの -c 文字列)も殺し、後続の命令が走らずに rc 144 で終わる(2026-09-25 に SSH トンネルを閉じるときに発生)
metadata:
  type: feedback
---

`pkill -f 'ssh -f -N -L 19890…'` を Bash ツールで実行したら、コマンド文字列そのものにパターンが含まれるため自分のシェルが SIGTERM で落ち(rc 144)、同じ行に続けて書いた EC2 の API 確認が走らなかった。`pgrep -af` も同様に自分自身に当たり「already」と誤判定した。

**Why:** Bash ツールは 1 行のコマンド文字列を `bash -c` で回すので、`-f`(全コマンドライン一致)は必ず自分に当たる。

**How to apply:** プロセスを止めるときは `pgrep -x ssh` で PID を取り、`/proc/<pid>/cmdline` を読んで引数で選んでから `kill` する。`pgrep -af` の結果から自分を除くか、`-x` で実行名を限定する。止める命令と、その後の確認は別の呼び出しに分ける。[[criterion-not-tool-output]]

- 2026-09-30 再発: `pgrep -f "<ssh の引数>"` が自分の bash(コマンド文字列に同じ語を含む)にも当たり、ループ内の kill で自分のシェルが落ちた(exit 144)。トンネルは先に kill できていたので実害なし。**`pgrep -x ssh` + /proc/<pid>/cmdline で選ぶ**、を今度こそ守る。
