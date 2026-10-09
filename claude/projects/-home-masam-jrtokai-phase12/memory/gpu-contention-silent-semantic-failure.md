---
name: gpu-contention-silent-semantic-failure
description: GPU を使う kg_api の子を 6 本並行にすると意味照合が黙って落ち、応答は正常な「語彙外」と同じ文になる・同時は 2 本まで・回ごとに台の健康を照らす
metadata:
  type: feedback
---

2026-10-05(決182 第 2 段 第 2 項の受入)、木 1,327 本 × 6 回を並行で回したら、変更前の回だけ意味照合が落ちた(同梱 diagnose が健康な回と 290/1,327 しか一致せず・1 位が違う 78・matched_symptoms が空 346 vs 50)。別の子は torch の CUDA の誤りで終了(GPU の取り合いと推定)。応答に出るのは「事象文からDKB症状語に照合できず(語彙外の可能性)」だけで、落ちた印は無い。負荷では STOP_BUDGET(予算切れ)も出る。

**Why:** 変更前と変更後の比較の差が実装の差に見えた(K4 が 279/1,316)。どちらの回が健康かは、別の健康な回と同梱 diagnose を照らして初めて分かった。
**How to apply:** GPU を使う子(kg_api の diagnose・木)は同時 2 本まで。各回の同梱 diagnose を健康な回(または相手の回)と照らす「台の健康」の数を判定に入れる。予算切れの木は比べる照合から外し、静かなときに回し直す。関連 [[c-grounding-silent-failure]] [[storage-order-changes-dkb-responses]]
