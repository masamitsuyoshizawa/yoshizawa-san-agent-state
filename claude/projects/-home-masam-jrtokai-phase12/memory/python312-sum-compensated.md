---
name: python312-sum-compensated
description: Python 3.12 以降の sum() は補正付き(Neumaier)で、浮動小数点の足す順の違いがまず結果に出ない・手で (a+b)+c と足して境目を探すと誤る
metadata:
  type: project
---

2026-09-28(dkb・決121): D-KG の照会の順の試験で、旧コードの G4(利得の元の確率の和)に陽性の対照が作れなかった。原因は **Python 3.12 から `sum()` が補正付きの足し算(Neumaier)になった**こと(`sum([0.1,0.2,0.3])` と `sum([0.3,0.2,0.1])` がどちらも 0.6)。ローカル・EC2 の kg_api とも 3.12(2026-09-28 確認)。境目を探すとき `(a+b)+c` と手で足したので、この違いを見落とした。

**Why:** 「足す順に依る」の主張は、実際の足し方(`sum()` か、ループの `+=` か)で成否が変わる。B の E1(`add_scores` はループの `+=`)は依存が本物、D-KG の G2〜G4(`sum()`)は実質なし。
**How to apply:** 足す順の依存を主張・試験する前に、実装が `sum()` か `+=` か・Python の版を確かめる。陽性の対照は実装と同じ足し方で作る。関連: [[negative-test-check-reason]] [[claim-wider-than-implementation]]
