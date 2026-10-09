---
name: dkb-coverage-facets-fed
description: 決80(D-KB の検索カバレッジ拡大・facets 連携)の fed 分の案と、そこで見つけた制約 5 つ
metadata:
  type: project
---

2026-09-25 決80(利用者指示: D-KB の検索の基本は equipment_class・location_class / same_equipment は特異点として注意喚起)。fed の案 = docs/検討_D-KB検索カバレッジ拡大_fed分_20260925.md(fe980712f1f4b0d9・fed 4e384b73・rep 20260925-1646)。**実装していない(設計承認待ち)**。

見つけた制約(確定): (1) facets の 3 切り口はどれも現象を条件に含む → 現象が辞書に無いと same_equipment も走らない (2) 場所の規則が違う(facets 駅/構内/信号場・D-KB 駅/踏切/停車場/構内。LOO700 で 159 vs 427) (3) kg_api 内に B への経路が 2 本(KG_ACCIDENT_URI と KB_ACCIDENT_NEO4J_URI・EC2 で同一か未確認) (4) dkg_backend._b_example_causes は Cause.text(原文)を返す → 原因コードに使えない (5) search は 3 切り口を必ず全部照会 → 内部専用 facet_keys を推す。LOO700 で location_class 実行可 144・same_equipment 3。費用: 抽出/計画 3 ms・照会は 1 組 6.7 ms(段 5 から推定)。

**Why:** D-KB と facets の統合は、同じ語でも規則・経路・条件が違う箇所が多い。**How to apply:** 設計承認が出たら、この 5 つを実装の前提に置く。場所の規則を変えると facets@v4 の答えが変わる(凍結・承認)。関連 [[v3-plan-b-stage1-complete]] [[dkb-recurrence-improvement]]

**決81(2026-09-25 16:57)と fed の計画**: docs/計画_facets_マスク47と段1案C_fed_20260925.md(db4fbae39eab3448・fed ac097efe)。第 1 部 諮5 = facets の _RETURN を coalesce(*_masked, 生) に(一致条件は不変・規則で置換・bkb の 47 件は照合用)+ 追補 1 = federate の B 枠 F1〜F4(一致は生の名・表示だけ置換)。**静的データ(効果デモ 435・仕様書の例 392 の place_text)は API を変えても生のまま → 作り直しが要る**。第 2 部 = facets の口 3 つ(equipment_source・facet_keys・照会の中で除く exclude_ids)で、欄を組むのは dkb(fed の singularities() は取り下げ)。第 3 部 = 4 つ目 same_equipment_any(要判断 3: 設備の一般名を入れる・組は場所と番号がそろうときだけ候補ごと(49→84 の 422 を避ける)・most_specific_facet に入れない)。**実装は coord のまとめての承認の後。**

**決82 の実装(2026-09-25 17:10 承認)**: (1) マスク 47 件 = facets の RETURN coalesce(917c0989)・federate F1〜F4 + group_line/group_places/display_name(5bbab9e2)・効果デモ作り直し(dba45157)→ **EC2 反映済み(coord info 1738・4 口 mask_field_check 合格)**。federate に規則外の RAW 12(source_file・マスク欄の無い名)→ 判断 (c) 地名か人名かの判定を急ぐ(coord が諮る)。**B は重複の群(DUPLICATE_OF)の相手・同名の別ノードにマスク欄が無い** — 欄ごとの規則では漏れる。(2) 段 1 = facets の口 3 つ + same_equipment_any(e86b399e・fed f32ca390・凍結 200)。exclude_ids は除外つきの版 CYPHER_EX(HTTP の文は不変)。**EC2 に置くと HTTP の facets に 4 つ目がすぐ出る**。仕様書の例は段 1 反映の後に coord が採り直し → fed が生成し直し。

**決86(2026-09-25 18:35)でマスクは撤回**: 利用者判定「すべて地名・人名 0」→ 場所・線区・名前は生の値。fed = facet_backend の coalesce を外した 03dc53a6e7bd73ee(段 1 の口 3 つと 4 つ目は残す・元の 3 切り口の文は S7 と同じ)・federation.py を 5aa6dd2e に戻す・E38 を生の値に・test_federation_mask82 撤去・効果デモは旧版 70b32e66 が正。**段 1 の EC2 反映で配るのは 03dc53a6(e86b399e は使わない)**。凍結試験は 206(固定例 F1〜F6 を足した)。
