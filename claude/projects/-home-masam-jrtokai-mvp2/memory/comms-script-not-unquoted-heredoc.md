---
name: comms-script-not-unquoted-heredoc
description: 連絡文を書く Python を引用符なしの heredoc で流すと、本文の逆引用符がシェルに解かれて字が抜ける(2026-11-18・dgr・info 2359)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-17T15:00:20.890Z
---

連絡文は、Write で置いたスクリプトの .py を回して書く。シェルの値(sha など)を入れたい時も、`<<EOF`(引用符なし)の heredoc に Python を流さない。

**Why:** 2026-11-18 に dgr が、MANIFEST の sha を入れるために `<<EOF` で Python を流した。本文の `` `data/...` `` がコマンドとして解かれて空になり、置き場の字が抜けたままコミットした(info 2359・直しのコミット d6dbbbb)。CLAUDE.md の「連絡文はシェルの heredoc で書かない」の抜け道になっていた。

**How to apply:** 値はスクリプトの中で求める(hashlib で sha を取る)か、引数で渡す。heredoc を使うなら `<<'EOF'`(引用符つき)だけにする。書いた後は、逆引用符の中の字が残っているかを grep で確かめる。[[remerge-main-before-writing-comms]] [[comms-time-from-date-not-rounded]]
