---
name: c-conn-mark-acceptance-bkb
description: 決183 甲(C 接続失敗の印)の受入 2026-10-06 合格(rep 0108)・F1 経路の無い口で diagnose 60〜150 秒・F2 ■ の位置・道具 c_conn_acceptance.py
metadata:
  type: project
---

決183 甲の受入(bkb)は 2026-10-06 に J1〜J15 全部計画どおり(rep 20261006-0108・証跡 knowledge_kb_v8/data/eval/bkb/c_conn/run_20261006-0023・版 a14a8cbe の写し)。
指摘: F1 経路の無いアドレス(パケットを捨てる口)で diagnose 1 回 60〜150 秒(失敗をキャッシュしないので毎回)・上限が要る。F2 guided 未収束の ■ の塊は guided の結びの行の前(計画は末尾)。
道具: c_conn_acceptance.py(写し・環境で失敗・中継の口 Relay・部品差し替え・SCAN で呼ばれる事象を選ぶ)・judge_c_conn.py・check_c_conn_plugin.py・check_tree_soak.py。
教訓: 場所の不足は input_gaps の kind=place(文字列「場所が未指定」では一致しない)。差し替えの試験は呼ばれた数を記録しないと成り立ったか分からない。次 = 丁(req 0102)・乙(req 2243)の受入設計。関連 [[tree-stage1-acceptance-bkb]] [[negative-test-check-reason]] [[main-tree-moves-during-run]]

2026-10-06 追記: 決192 で EC2 反映は F1 の是正(接続の待ち時間の上限・失敗後の再試行の間隔)の後。bkb は dkb の計画改訂の後に限定の再走(黙って捨てる口の所要の上限・間隔の間は試さず印・間隔の後に復旧・健康時 byte 同一)。F2 は fed が計画を実装に合わせる。乙(設計 bca3a7b0・rep 0111)・丁(設計 e02e4198・rep 0111)の受入設計は返済み・実走は各実装の rep の後。

2026-10-06 追記 2: 丁の受入 合格(rep 0152・版 d136d4a8・道具 tei_acceptance.py/judge_tei.py/check_tei_plugin.py・限定=症状と提案の両方の失敗は作れない)。F1 の限定の再走 合格(rep 0218・版 97ab77c9・経路の無い口 2.98 秒・間隔 30 秒・道具 f1_acceptance.py/judge_f1.py)。残り = 乙の受入(実装と評価の rep 待ち)。教訓: 甲の run2 を流用すると甲の道具を起動する・引数の中の評価は要求の前の値。

2026-10-06 追記 3: F1 再走 2 回目(F192-1・2・版 12e88c8b)合格 rep 0308。試行の数は CountingRelay(中継の口が受けた接続)で外から数える・stall の口。教訓: 所要の閾値は同時の要求の並行の負荷を入れてから決める(健康な C の対照を同じ形で回す)・結果を見てから閾値を替えたら前後を両方書く。残り = J20(抑止の間の文 :deferred の承認の後)・乙の受入。

2026-10-06 追記 4: J20(決194 :deferred)合格 rep 0841(e449b397)・甲の受入設計 改訂 3(対照つきの閾値)。乙の決定論の部分 合格 rep 0856(main 4dcc744b・道具 phase_acceptance.py/judge_phase.py・所見 = f 行の〈前の値〉の(復旧後))。甲・乙・丁の受入が揃い、coord が EC2 反映を諮る段。教訓: 第 1 項の行は前の手番を渡さないと prev_missing で出ない・Python の bool は int(数の検査に真偽の欄を混ぜない)。

2026-10-06 追記 5: 決198 甲乙丁の EC2 反映(kg_api タグ abc-20261006)の受入 合格 rep 0923。道具 ec2_abc_acceptance.py(コンテナ jrtokai-v9-kg-api の中で台本を `ssh kb-demo-ec2 "docker exec -i ... python3 -"` の標準入力で回す・ファイルを置かない・鍵は KG_API_KEYS)。EC2 とローカルの木の差は meta.flags.DKB_SINGULARITIES の報告だけ(既知)。
