---
name: check-exit-code-when-filtering-test-output
description: 試験の出力を grep で絞る時は終了の値を見てからコミットする(selftest 9/10 のままコミットした件・2026-11-17・acc)
metadata:
  type: feedback
---

試験(selftest)の出力を `grep -v PASS | tail` などで絞ると、パイプの終了の値は grep の物になり、試験が落ちても `&&` の先のコミットへ進む。acc は 2026-11-17 に、`scripts/acc_doc_tree.py` の直しを selftest 9/10 のままコミットした(e15dca1 → 6ddc32b で直した・落ちていたのは試験の中の見本の値)。

**Why:** [[pipefail-before-chaining-checks]] と同じ形の見落とし。漏れの検査では pipefail を付けていたが、試験の行には付けていなかった。

**How to apply:** 試験とコミットを 1 つの呼び出しにつなぐ時は、試験の行にも `set -o pipefail` を効かせるか、`${PIPESTATUS[0]}` を出して 0 を見てからコミットする。「selftest n/n」の字に FAIL が無いことだけで進めない。
