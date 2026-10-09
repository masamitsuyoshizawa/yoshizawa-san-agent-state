---
name: collation-derive-expected-from-source
description: 照合は上流(辞書・契約)から期待値を独立に作る。API の出力を数え直すだけでは、その出力自体の誤りは見つからない
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-21T15:02:59.235Z
---

実装の照合で「API の `result_match` から `counts` を独立に数え直して一致」としても、`result_match` 自体の誤りは見つからない。V3 段 6(2026-09-21)で coord は「全 5,843 語を入口に通して不一致 0」と報告したが、見たのは `query_match`(質問の側)だけで、結果の側の誤り 2 件(当たった語に、その grouping の展開が採っていない所属が付く・現象の `term_role` が null)を rev が静的な導出で見つけた。桶が同じなので件数は変わらず、件数の一致では見えなかった。

**Why:** 検算の向きが下流に偏ると、上流の誤りが素通りする。「N 語で不一致 0」という大きな数は、見た範囲より広い安心を作る。

**How to apply:** (1) 由来や集合は、生のデータ(辞書)→ 選別 → 展開 → 当たり → 帰属 → 集計 を一段ずつ、API を使わずに作って突き合わせる。(2) 「N 件で不一致 0」と書くときは、どの欄を見たかを同じ文に書く。(3) 語を単独で引いた結果を「質問に届く」という主張にしない(質問の全文を入口に通す — 同日に 2 回: A1 の材料と是正の全体)。(4) 否定例は、狙った枝に到達し、正しい実装と誤った実装で期待値が違うことを先に示す(片側の鍵だけ壊した変異は、意味の違反の検出ではない)。(5) 後の段で欄が増えたら、古い門(凍結・スキーマ)へ戻して検査を足す。関連: [[claim-wider-than-implementation]] [[report-from-the-consumer-side]] [[negative-test-check-reason]]
