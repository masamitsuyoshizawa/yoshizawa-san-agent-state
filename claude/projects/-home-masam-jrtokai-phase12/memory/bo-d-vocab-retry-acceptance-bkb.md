---
name: bo-d-vocab-retry-acceptance-bkb
description: 決230 戊-d(語彙 DB の再試行・束のロック待ち)の bkb 受入 — fed 分 D-1 合格(rep 20261010-1235)・facet_backend.py は読まずに振る舞いで見る型・D-3 は dkb の後
metadata:
  type: project
---

2026-10-10 決230 戊-d の fed 分 D-1(5a63b349・facet_backend 38d573e4)受入 合格(rep 1235・道具 bod_acceptance.py / judge_bod.py・証跡 bod/run_20261010-1215)。

- facet_backend.py は bkb が中を読まない取り決め → 語彙 DB の向け先(KB_VOCAB_NEO4J_URI)を、閉じた口・受け付けて返さない口(受け付け数を数える)・10190 への中継・遅い中継に替えて振る舞いだけ見た。
- 失敗 1 回目 2.6〜5.1 秒(前 31〜121 秒)・直後 30 秒は 0.01 秒で deferred(受け付け増えない)・30 秒後の同時 5 本は読み込み 1 回分(待った 4 本は束のロックで最長 5 秒待って deferred = 計画 §3-3 の設計)・ロック待ちは遅れ 0.3 秒の中継(読み込み 25 秒)で 10.02 秒 deferred。
- 判定の誤り 2 つ(reader_reason の型の名を漏れと数えた・所要で「試した」を数えた)を計画の文言で直した。最初の健康の初回 5.6 秒は写しの直後の 1 回目で、交互 3 回で新旧とも 1.4 秒前後。
- 残り: D-3(sources_down・deferred の文・dkb)の受入、EC2 は 3 件まとめて別承認。

**Why:** 振る舞いの検査で「待ち」と「試し」を所要で区別すると、ロック待ちの設計を誤って不合格にする。
**How to apply:** 同時の試行は外から数えられる量(受け付け数・理由の札)で数える。関連 [[query-timeout-bo-bkb]] [[test-app-lacks-prod-error-envelope]]

2026-10-10 13:22 決230 dkb 分 3 件(14e1d9bc)受入 合格(rep 1322・bo230_acceptance.py・bo230/run_20261010-1301)。不変 288/288・接頭辞の後ろ盾は L2 を前の観測を空で再現(前の版 = 一覧に無い質問)・_field_checks は止めた口で空 → 復旧後 475 件・陽性 5/5(前は lru_cache で空を保持 0/5)・D-3 は failed → deferred → 印なし(1 回目の failed を見るには木の準備より先に diagnose を呼ぶ)。木の枝は sources_down を持たない。残り = EC2(3 件まとめて別承認)。

2026-10-10 14:29 R230-1/2 の是正(4ab2eb6b・tree_state 9176db82)の回し直し 合格(rep 1429・r230_acceptance.py)。木の logger は要求につき 1 回。

2026-10-10 15:02 決231(決230 の EC2 反映)EC2 受入 合格(rep 1502・ec2_bo230_acceptance.py)。**EC2 の kg_api は uvicorn --log-level warning でアプリの INFO が docker logs に出ない** → logger の件数を使う設計(決230 P-2)は EC2 で数えられない(所見・低)。コンテナ内の別プロセスで INFO にして関数を呼ぶ形で代えた。決230 の bkb 受入はこれで全部済み。

