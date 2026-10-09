---
name: fed-vs-dkb-comparison
description: federate/lite vs D-KB比較評価(層化50問・LOO同一条件)の結果と改善示唆
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-08T15:43:48.273Z
---

2026-09-09完了。層化5層×10問・exclude_idsで3方式同一LOO条件・ブラインドLLM4値判定。
結果: c+p=federate70%/lite68%/dkg48%(dkgはLOO700実測46.2%と整合)。時間中央値=dkg0.1s/lite26.7s/federate41.9s。
リーク検査で除外漏れ0件を確認(gold12字一致8件は全て正当=入力文由来1/頻出設備名3/別事故の同一原因4)。
federate系の勝ち筋=B全文検索による同一箇所・同一設備番号の再発事例発見。初見層は逆にdkgのHB規範が勝つ例あり。
liteはfederateと僅差で1.6倍速(C系は本サンプルでほぼ発火せず)。
改善示唆(未実施): 駅名・設備番号一致でB事例を直接照合する決定論パスをD-KBへ追加すればfederate勝ち筋の大半を0.1sで回収可。
レポート=knowledge_kb_v8/reports/federate_vs_dkb_comparison.md、スクリプト=scripts/dict/run_fed_vs_dkb.py(sample/dkg/lite/federate/judge/report)。
関連: [[dkg-cause-kg]] [[federate-latency-optimization]]

**Why:** 顧客提案・役割分担設計(初動=dkg/二次調査=lite)の実測根拠。
**How to apply:** 方式選定や精度言及時はこの実測値を引用。再評価はrun_fed_vs_dkb.pyを再実行(スナップショットはgit外data/eval/fvd_*)。
