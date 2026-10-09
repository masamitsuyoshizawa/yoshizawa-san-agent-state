---
name: c-grounding-silent-failure
description: diagnose の C-KG 接地は認証の失敗を握りつぶし grounded_equipment が空になるだけ・受入の台に C の接続を入れ、回す前に空でない件数を確かめる
metadata:
  type: feedback
---

dkg_backend の C 系の接地(dkg_grounding・kb_conn の "c" = KG_C_URI・KG_C_PW)は、接続や認証に失敗しても例外を握りつぶし、`grounded_equipment` が空になるだけで応答に印が残らない。「駅・踏切名が文中にない」と区別が付かない。

**Why:** 2026-10-05、bkb の受入の台(run_tree_s1_acceptance.base_env)に C の設定が無く、第 1 段の受入の全回で C の接地が失敗していた(受T1 の 2,654 応答で空でないもの 0)。新旧の両側が同じ状態だったので差 0 は崩れないが、接地の効く経路を確かめていなかった。dkb も同じ型で 10-02 の写しとの 62 件差を出した(rep 1846)。
**How to apply:** D-KG の diagnose・木を回す台には KG_C_URI=bolt://localhost:9990・KG_C_PW(neo4j-crossing-v6)を入れる。回す前に、LOO の先頭など数十件で grounded_equipment が空でない件数が 0 でないことを確かめる。比べる両側が同じ環境でも「効いていない経路」は確かめたことにならない。関連 [[silent-path-drift-masks-cause]] [[tree-stage1-acceptance-bkb]]
