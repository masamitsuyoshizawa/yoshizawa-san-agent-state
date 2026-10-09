---
name: main-tree-moves-during-run
description: 受入を本体ツリーで回すと他セッションの merge でコードが途中で替わる・固定版の写し(make_base で commit に戻す)で回し、写しの sha16 を照らす・記録が実際に回した版を指すか確かめる
metadata:
  type: feedback
---

本体ツリー(/home/masam/jrtokai-phase12)は main を checkout しているので、他セッションの ff merge で kg_api やプラグインが走っている最中に替わる。2026-10-05 の第 2 項の受入中に、甲(dkg_backend 等)とプラグイン 0.3.13 が入り、後から始める段が別の版になるところだった(coord の指摘)。

**Why:** 受入は「どの版で合格したか」が EC2 反映の根拠になる。版が混ざると証跡が無効。さらに E2 の versions.json は prep が本体ツリーを記録する作りで、写しで回した証跡にならなかった。
**How to apply:** 受入の開始時に固定版の写しを作る(tree_s2_item2_acceptance.make_base(out, <commit>, name) + deploy のプラグイン・DSL を git show で置く)。変更後の側は BKB_AFTER_ROOT・--p12・BKB_DSL_ROOT で写しを指す。写しの全ファイルの sha16 を固定版と照らす。走っている間の main の reflog と kg_api の変更の時刻を突き合わせて、各段の開始時刻と並べて rep に書く。関連 [[audit-run-in-main-tree]] [[tree-stage1-acceptance-bkb]]
