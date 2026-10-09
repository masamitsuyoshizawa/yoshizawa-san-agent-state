---
name: v3-stage5-api-state
description: V3 第 1 段の API 実装(fed)は段 5 まで完了・閉じ済み。段 6 は coord の別便待ちで、_absorb の status/source は直さない
metadata: 
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-21T13:30:09.921Z
---

**V3 第 1 段の API(`kg_api/sources/facet_backend.py` / `facet_router.py`・担当 fed)は、2026-09-21 に段 5 まで完了し、coord が照合して閉じた**(`docs/comms/20260921-2227_coord_to_fed_rep_stage5-closed.md`)。

- **段 5 の内容**: 切り口 3 × `grouping` 2 = **6 組**・`options.groupings`(契4)・`expanded_terms`。是正コミット **66c28645**。
- **試験**: `test_facets_v3_freeze.py` **48 件**・`test_facets_v2.py` 23 件・`test_vocab_digest.py` 6 件。
- **所要の実測**(ローカル B 9890・700 件・上限 10・`all`・5 回の中央値・同一バッチ): **3 組 20.8 ms / 6 組 40.3 ms**。**別の日の 37 ms とは比べない。**
- **既定の `alias_policy` はまだ `all` ではない**(段 7 で変える)。**既定のままだと発端の質問は 6 組とも `skipped`(`not_in_vocabulary`)になる** — 所要や挙動を測るときは `load_vocab(alias_policy="all")` を使い、`status` が `ok` であることを先に `assert` する。

**次は段 6(`query_match` / `result_match` / `evidence_breakdown`)だが、coord の別便を待つ。** 保留の理由は、`_absorb` が**正準名を entry の `status` に依らず `confirmed`・`source` = `["canonical"]` として登録している**件。設備辞書 `cf5bc4b2bf573cf1` の entry は proposed 2,405・欄なし 1,145・confirmed 0 で、`["canonical"]` は辞書に無い値(共通表 §1-2 に反する)。**表示の規則を利用者に諮っている。いまは直さない。**

**段 6 で扱うと決まっているもの**: `_matched()` が `extract` の語を先頭から見て最初の 1 語で止まるため、**`grouped` の子孫の語で当たった事故の `matched` が空になりうる**。段 6 で `result_match` へ置き換えるときに扱う。

**確定した限定**: **`expanded_terms.count` は「展開した語の数」であって、独立に効く語の数でも当たった語の数でもない**(照会は `CONTAINS` なので、`182イ分岐器` のような語は `分岐器` に包まれ結果を増やさない。strict 15 語中 11・grouped 92 語中 59 が該当)。**辞書も展開も変えない。**

関連: [[v3-stage1-design-approved]]・[[v3-stage1-acceptance-state]]・[[claim-wider-than-implementation]]・[[negative-test-check-reason]]
