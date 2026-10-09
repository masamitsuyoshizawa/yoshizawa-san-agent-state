---
name: facets-input-regex-pitfalls
description: facets/diagnose の入力解釈の落とし穴(場所 regex の左境界なし・「1号線」・「々ー」・現象語の表層・finding は「正常」「異常」の 2 語)。2026-09-28 に実装と EC2 で確かめた
metadata:
  type: project
---

`kg_api/sources/facet_backend.py` の `_RE_LOCATION = ([一-鿿ぁ-んァ-ヶA-Za-z0-9]{1,8}?)(駅|構内|信号場)`・`_RE_EQUIP_ID = ([0-9０-９]{1,4})\s*号`(2026-09-28・main e51b0d92)。`re.search` で最初の 1 つだけ。
- **左境界が無い**: 「過去に新城駅で…」→ 場所 `過去に新城駅`。「JR名古屋駅」→ `JR名古屋駅`。場所を要する切り口が ok の 0 件になり気づきにくい(bkb 指摘・coord が Python と EC2 detail=list で再現)。→ 場所名は文頭か読点・空白の直後。
- 「1号線」の `1号` が設備番号に取られる。「々」「ー」は文字の組に無い(「代々木駅」→ `木駅`)。
- 現象は辞書表層の部分一致: 「…の転換不能」は現象(正準名 不転換)として取れるが「…が不転換」は not_in_vocabulary(設備クラス側は「転てつ」が別名で当たる)。「転換不良」は「不良」→ 故障に流れる。
- D-KB `dkg_backend.py`: finding から候補を作る所(397・424 行)は `result == "異常"` の文字どおりだけ。`abnormal`/`基準外` は支持・反証にしか効かない(dkb 指摘)。症状照合は `federation.dkb_match_symptoms` の部分一致(長い語が先・最大 3)→ 意味照合 → 辞書展開の 3 段。

**Why:** LLM を使わない入力処理は、文字列の形にそのまま依存する。ガイドに書く規則は実装の正規表現と実応答で確かめる。
**How to apply:** 入力の指針を書くとき、正規表現は境界(左右)と文字クラスまで読み、例文を実際の口に投げて interpretation を見る。関連 [[agent-docs-bkb-dkb-20260928]] [[v3-plan-b-stage1-complete]]
