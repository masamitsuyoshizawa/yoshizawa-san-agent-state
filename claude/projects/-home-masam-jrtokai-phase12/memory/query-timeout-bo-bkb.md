---
name: query-timeout-bo-bkb
description: 戊(照会の待ちの上限・決193/199)の bkb 分 — 受入設計 f0e5bc01・accident_backend の err の保持の判断材料・戊-a の検証を担える
metadata:
  type: project
---

2026-10-06 決199: bkb は (1) 戊-b(案 D・案 E)の受入設計 改訂 0(docs/設計_受入_照会の待ちの上限_bkb_20261006.md f0e5bc013b3a62af・Q0〜Q10・中継の口に pause = 接続を保ったまま転送を止めるを足す・手元の合図は 120 秒)を返した。(2) 共有 accident_backend の `_STATE["err"]` はプロセスの間ずっと保持され B の口 5 つと dkg の B 枠が回復しない・warnings に str(e) が出る経路あり → 推し = 30 秒の間隔で 1 本だけ試し直す・固定の文(info 1118)。(3) 戊-a の B の写しでの案 A の検証(keep-alive 5 秒 × 2・演習の投入・監査・書き出し・並行)を担えると返した(rehearse_env の一時の Neo4j に設定を渡す口が要る)。実走は実装の rep の後。関連 [[c-conn-mark-acceptance-bkb]]

2026-10-06 決200: 己(案 3・固定の文「B(事故摘録の正本)に接続できていない」)と戊-a の B の写しの検証は bkb の担当。計画 docs/計画_己とB写しの案A検証_bkb_20261006.md(d7ce4ee5)・dkb へ合意の req 1130(点 1・3)・fed へ SourceB._c の req 1130(点 2)・点 4 = dkb が rehearse_env に --neo4j-env を足す(合意済み)。合意の後に LOCKS を取って実装。fed の戊-a(語彙 DB)へ bkb の長い処理の一覧を rep 1131(全部読み取り・d2 がそのまま写しへ向く)。

2026-10-06 12:28: 己を実装・main f596ea20(accident_backend b7c7ad79・router e79990fb・client 1f6f6d50)・受入 K1〜K8 合格(rep 1228・道具 ki_acceptance.py/judge_ki.py)。教訓: CountingRelay の既定の転送先は C 9990 → B では target を渡す・worktree の道具で子を起こすと git 外のデータが無い(本体ツリーから回す)。次 = 戊-b の受入の実走(req 1214・案 E で C の残りの入口は :skipped)・戊-a の B の写し(rehearse_env --neo4j-env あり main 0caa8b5b)。

2026-10-06 13:00: 戊-b 受入の実走中(道具 bo_b_acceptance.py・judge_bo_b.py・置き場 knowledge_kb_v8/data/eval/bkb/bo_b/run_20261006-1240・写し main ce962308)。Q0 H=4 正本とも 120(語彙 DB も)・Q6 は表どおり。教訓: LOO の先頭と木の 22 問は C を呼ばない → 断の台は健康なときにその正本を呼ぶ事象を中継の記録で選ぶ(select 段)・代理の session.run は引数 q= を名で受けるので *a で通す。庚+辛の道具 ko_shin_acceptance.py(変更前 4 本: docs_router e12dd014^・docs_backend e57ae8c6^・dkg_router 2798e33f^・federate_router 16f44c8a^)・戊-a の B の道具 bo_a_b_copy.py は準備済みで未実走(戊-b の後・戊-a は LOCKS 18687/18688 と info を先に)。

2026-10-06 15:00 完了: 戊-b 受入 Q0〜Q10 合格(rep 1430・写し ce962308)・庚+辛 受入 合格(rep 1441・写し edbda372・道具 ko_shin_acceptance.py)・戊-a の B の写し(rep 1459・合図 10・cypher-shell の Java driver 6.0.2 も合図を読む・切断 0)。残り = rev の実装確認 1 巡のみ。
教訓(道具): 中継の口の「2 回目」は接続と長さで数える(切れた瞬間に driver が同じ宛先の他の接続へ GOODBYE 6 バイトを送る)・黙った断を試す事象は健康なときにその正本を呼ぶものから選ぶ・同じプロセスの中継のスレッドは名で除いて数える・内部の札 unavailable は型の欄にも正当に出るので warnings の中だけで数える・この台の docker はホストの中継の口に届かない(黙らせるのは docker pause)・unmask は正本に適用済みで門 G16 が止める(演習の積荷に使えない)・出力先の親が無いとシェルのリダイレクトで回る前に落ちる。

2026-10-09: 戊・己・庚・辛の受入はすべて閉じた(rev 1512 の穴 R1〜R4 = rep 2038/2040・EC2 第 1 部 = rep 2215・案 A は決208 で EC2 本採用済み)。rev 2044 注意1(比べない欄の範囲)も直した(info 0119)。手元の B は 360406c6・27,267/46,793。次の bkb の受入 = 表示改善 第 3 段(決207 (B)・fed の計画と dkb の API 案の後)。置き場: bo_b run_20261006-1240(改訂 0)・run_20261006-1957_r1(是正の後)・ko_shin run_20261006-1431・run_20261006-1957_r1・ec2_bo run_20261006-2209。
