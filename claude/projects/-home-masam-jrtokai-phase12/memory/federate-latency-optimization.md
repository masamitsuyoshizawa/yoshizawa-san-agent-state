---
name: federate-latency-optimization
description: federate統合(fused)が1件約300秒と遅い。軽量化が宿題(利用者依頼)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-07T10:42:09.689Z
---

federate の統合合成(`/v1/federate` mode=fused, lead=A)は本番EC2インスタンスで**1件あたり約300秒**かかる。原因: A(BGE-M3索引の複数RAG検索、1検索30〜60秒)+ B診断 + C接地 + 合成LLM(opus-5既定/sonnet-4-6でも重い)を直列実行するため。

利用者依頼(2026-08-07): この**動作軽量化を宿題として記録**。当面の暫定対応として v10デモの api() 既定タイムアウトを240→500秒へ引き上げ済(コミット f6984c7)だが、体感の遅さは未解決。

**Why:** 実運用・デモで federate を多用するとタイムアウト/待ち時間が問題になる。評価API([[abc-unified-api]])やQ3/Q4再評価の文脈で発生。

**How to apply:** 改善候補 — (1)合成の非同期化(ジョブ投入→ポーリング)、(2)A検索の索引ウォーム維持(遅延ロード解消・常駐)、(3)RAG検索の並列化/topk削減、(4)合成モデルの軽量固定(sonnet系)+max_tokens調整、(5)sources既定からC除外。着手時は [[eval-and-table-comprehension]] とは別系統(B/A事故摘録側)である点に注意。
