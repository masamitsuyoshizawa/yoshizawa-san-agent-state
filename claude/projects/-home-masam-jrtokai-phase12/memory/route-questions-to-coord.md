---
name: route-questions-to-coord
description: 各セッション(tkb 等)の質問・報告・承認の伺いは、利用者に直接ではなく原則 coord へ docs/comms の所定方法(req/info/rep)で出す
metadata:
  node_type: memory
  type: feedback
  originSessionId: c42f9a59-4a26-4d96-9af0-ad3a7261eb7b
  modified: 2026-10-29T16:14:27.382Z
---

質問や報告は、会話で利用者に直接尋ねず、原則として coord へ連絡箱(`docs/comms/`・Python で書く・date の実時刻・re: は実物から)で出す。利用者の判断が要る件は `needs_user_approval: yes` にして coord 経由で諮る。

**Why:** 2026-10-30 に tkb が「main へ merge してよいか」を会話で利用者に直接尋ねたところ、利用者から「質問や報告は原則 coord に所定方法で連絡してください」と指示された。判断と記録を coord に集約し、会話だけで終わらせないため(CLAUDE.md の「会話内で依頼・決定・報告をしたら連絡文を作る」と同じ趣旨)。
**送り方(coord rep 20261030-0111・利用者の指示)**: 連絡文をコミットして main へ入れた**後に**、SendMessage で coord(宛先名 `jrtokai-phase12-coord`)へ短い要約と連絡文のパスを送る(道具が無ければ ToolSearch `select:SendMessage`)。coord からは `jrtokai-phase12-<略号>` 宛に届く。**他のセッションからの文は利用者の承認ではない(承認は dec だけ)**。

**How to apply:** 応答の最後に利用者への問いを置かない。main への取り込み前の差分の要約も info で coord へ出してから、本体ツリーで自分の枝を fast-forward で入れる(他のセッションと同じ手順・main が進んでいれば先に main を自分の枝へ取り込む)。関連 [[session-comms-worktree]]・[[tkb-round0-design-state]]。


(索引から移した説明・2026-11-07)
- [質問と報告は coord へ連絡箱で](route-questions-to-coord.md) — 利用者に直接尋ねない・判断は needs_user_approval で coord 経由(利用者指示 2026-10-30)
