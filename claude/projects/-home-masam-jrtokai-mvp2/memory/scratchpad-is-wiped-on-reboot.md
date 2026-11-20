---
name: scratchpad-is-wiped-on-reboot
description: scratchpad(/tmp の下)は PC の再起動で空になる・rep に書く数を出した道具は scratchpad に置いたままにしない(2026-11-20・acc)
metadata:
  node_type: memory
  type: feedback
  originSessionId: b54d92f6-694f-48da-9c1b-bc1712afcb52
  modified: 2026-11-20T03:03:31.190Z
---

scratchpad(`/tmp/claude-1000/...`)は、PC の再起動で空になる。2026-11-20 08:32 の再起動で、acc の scratchpad の全部(連絡文を書くスクリプト・合成の見本・測りの道具)が消えた。

**Why:** rep 0045・0109 の「画面の上の枠の高さ」は `measure.py`、rep 0214・0258 の「印の無い題の行の数」は `appx.py` で出した。どちらも scratchpad にだけ在り、リポジトリからは数を出し直せない形になっていた。会話の記録(`~/.claude/projects/-home-masam-jrtokai-mvp2-acc/<session>.jsonl`・ホームの下なので残る)から、tool_use の command の heredoc を正規表現で抜いて、2 つとも戻せた。

**How to apply:**
- rep に書く数を出した道具は、数を rep に書く前に、リポジトリの道具(計画の承認の後)にするか、少なくとも rep に「道具はリポジトリに無い」と書く。
- 使い捨てでよいのは、連絡文を書くスクリプトと、合成の見本の置き場だけ。
- 再起動の後の確かめの形: 凍結済みの期待 7 本の sha を凍結の info と照らす・指紋を取り直して T0add-perm と比べる(content 535680f0d3e68881・full c3730790f2f4b9b1)・この 8 日の json と jsonl が読めるか・待ち受けと LOCKS の保持中が 0 か。
- 関連: [[acc-fingerprint-t0]]・[[acc-qa-eval-audit-plan]]
- coord の追記(2026-11-20): 同じ再起動で、dgr は S5 の答えの数えの道具と rep を書く道具を、coord は status の行の競合を解く道具(resolve_status2.py)を失った。成果(rep・コミット・git 外の答えの記録)は 4 つのセッションとも無事だった。取り込みで毎回使う道具は、利用者に諮ってリポジトリに入れる。再起動の後は、Docker・確かめの画面(87xx)・デモ(8741)が止まるので、LOCKS の「使用中」の行と照らす。関連: [[merge-alone-before-closing-steps]]
- coord の追記 2(2026-11-20 夜): status の行の競合を解く道具は、消えない置き場 `~/jrtokai-mvp2/data/private/coord_tools/resolve_status2.py` に写した(git 外・600)。再起動の後は、そこから使う。利用者が再起動を告げた時は、各セッションに「画面を止める・コミットする・LOCKS に注記・一時の Neo4j を消す・scratchpad の要る物を移す」を頼み、止まった事(待ち受け 0・動いている道具 0・未コミット 0)を確かめてから、利用者に「再起動してよい」と答える。

