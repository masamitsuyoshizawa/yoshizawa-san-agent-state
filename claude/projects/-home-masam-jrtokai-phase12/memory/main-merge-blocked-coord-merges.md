---
name: main-merge-blocked-coord-merges
description: 担当の枝の main への早送りと push が自動の許可の判定で止められたら、回り込まずに枝だけ push し、止められた事と枝のコミットを coord に知らせる(coord が中身を確かめて取り込む)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-19T03:16:39.521Z
---

担当のセッション(bkb など)で、本体ツリーへの `merge --ff-only` と main の push が自動の許可の判定(auto mode classifier)で止められた時は、判定を回り込む工夫をしない。自分の枝だけを push し、**止められた事と枝のコミット**を coord に連絡箱と会話で知らせる。coord が中身(ファイルの種類・データや秘密が無い事)を確かめて main へ取り込む。

**Why:** 利用者の決め(2026-11-19・決351 追補 129・130)=「coord が取り込む(今後も)」。止められた操作を別のセッションが代わりに行うと判定をすり抜けるので、coord は利用者に諮ってから決めた。止められた事を隠さずに書いたので正しい道で決められた、と coord が評価した。

**How to apply:** 枝のコミットと push は自分で行う。**main への早送り(`git -C <本体ツリー> merge --ff-only`)は、単独の命令でも止められた**(2026-11-19 13:37・理由 Undermining Oversight・1 度目は push と合わせた命令で Destructive)。以後は試さず、枝のコミットを coord に知らせるだけにする。関連: [[session-comms-worktree]]
