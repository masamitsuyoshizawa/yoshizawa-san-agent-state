---
name: grep-shim-count-blank-on-cp932
description: このシェルの grep は関数(command grep ではない)で、Windows の .exe(netstat など)の CP932 の出力に -c を当てると 0 も件数も出さず空になる
metadata:
  type: reference
---

2026-11-01 tkb が口の空きを確かめたとき、`netstat.exe -ano | tr -d '\r' | grep -cE ':10290...'` が空を返した。対照の 9890 でも空だったので気づいた(確定・実測)。

- `type grep` は関数(Claude Code の shim)。UTF-8 の入力では `grep -c` は 0 も含めて正しく数える。
- Windows の .exe の出力は CP932(`file` で Non-ISO extended-ASCII)。これに `grep -c` を当てると、当たっても当たらなくても何も出さない(rc は 1)。`command grep -c` なら正しく 2 を返した。

**How to apply:** Windows の .exe の出力を数えるときは `| wc -l` か `command grep -c` を使い、必ず当たるはずの対照(既知の口など)も同じ式で数える。空の出力を 0 と読まない。関連 [[verify-zero-counts-before-reporting]] [[windows-hyperv-port-exclusion-blocks-docker-publish]]


(索引から移した説明・2026-11-07)
[grep の関数は CP932 で -c が空](grep-shim-count-blank-on-cp932.md)(netstat.exe などは wc -l か command grep)
