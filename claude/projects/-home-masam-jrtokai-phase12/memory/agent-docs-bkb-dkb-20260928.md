---
name: agent-docs-bkb-dkb-20260928
description: 2026-09-28 利用者指示で作ったエージェント向け資料 3 本(B-KB query/facets ガイド・D-KB dkg/diagnose ガイド・改変の説明)の所在と状態
metadata:
  type: project
---

利用者の指示(2026-09-28・「Dify・V10・shirei 稼働確認」の後): B-KB の accident/ask の代わりに query・facets を使うガイド、D-KB dkg/diagnose の改変込みガイド、改変の説明文。エンドポイント・パラメータ・各出力項目の丁寧な説明・curl サンプル。

- `docs/ガイド_B-KB_query_facets_エージェント向け_20260928.md`(初版 97a9237751dad4fc)
- `docs/ガイド_D-KB_dkg_diagnose_エージェント向け_20260928.md`(初版 a73eca9c327f2900)
- `docs/説明_B-KB_D-KB_改変の説明_20260917-28.md`(初版 150d8221bd50ba10)
- 出どころ: コード main e51b0d92・完全版仕様 e53f259c・DKG 仕様 20260907・EC2 実応答(2026-09-28 12:48・scratchpad に私有 0600・要約文/原因文/本文は「(省略)」に伏せて掲載)。
- 現物で確かめた点: `with_limit` は LIMIT が無いときだけ付ける(LLM の LIMIT 30 が効いた例)・search は Neo4j fulltext `accident_ft`・evidence_per_group 既定 1・singularities level 6 値。
- レビュー依頼: bkb(A)・dkb(B)・fed(C と A §2)= req 20260928-1301 ×3。合格後に利用者へ。Artifact 化は未提案。

**Why:** ask(2 LLM 呼び・21 秒)→ query(1 回)・facets(0 回・決定論)への移行をエージェント開発者に示す。
**How to apply:** 資料を直すときは実応答を再採取し(collect_samples.py の型)、sha16 と版を併記。関連 [[direct-path-content-filtered-item]] [[v3-plan-b-stage1-complete]]

**第 2 版(13:1x)**: レビュー全採用(bkb 5+6+1・dkb 1+6・fed 7)。sha16 B-KB c37d02da45450a41・D-KB bdf2370dba95347b・説明 98c154b3abda99cd。学び: grouping は下位語(子孫)へ広げる・total/returned は outline で 0・timeout_ms は照会 1 本ごと・max_rows は run_read が打ち切る・「順位不変」は誤り(同点の並びで答え 152/588 問変化)・facets 完全版仕様の also_in_scope は古い(fed へ申し送り)。fed/dkb の再確認待ち。

**最終版(2026-09-28 13:1x・main 4eeaecc0)**: B-KB c37d02da45450a41・D-KB bdf2370dba95347b・説明 0662928749b3618d(第 3 版・改変 3 の日付を 09-16 に)。bkb 修正のうえ合格・dkb 合格・fed 合格。also_in_scope の仕様側の是正は fed が別途。

**追記(13:2x)**: fed が完全版仕様 0925 版の also_in_scope を直した(982c430617b9b38a・1e07776e)→ B-KB ガイドの出典行を更新し第 3 版 7372fc839649c8ea(main 8df35953)。

**入力の留意点 追加(13:2x・利用者追加指示)**: facets §1-8(7 規則)・query §2-5(5 規則)・diagnose §1-2(12 規則)を実装から起こした(B-KB 0703df34a8d97927・D-KB f77a2f59544e33fa・main 5ae65a4c)。bkb/dkb に確認 req 1328。Artifact(3 タブ・marked 4.3.0・scratchpad/artifact/kb_api_guide.html)を公開。実装の要点: 場所 regex (駅|構内|信号場) 最初の 1 つ・番号 NN号 最初の 1 つ・NFKC 部分一致で全候補・question text は完全一致/20 字前方一致・answer 6 語・result 語集合・asked 完全一致。

**入力の留意点 第 2 版(13:4x・main 9cb1740d)**: B-KB 2a4f475f98ab9817・D-KB cd0683bd5fe7c5c8。実測で確定した規則: 場所 regex に左境界が無く「過去に新城駅」「JR名古屋駅」を場所に取り込む(→ 場所を文頭/読点直後に)・「1号線」が番号に・「々」「ー」は文字の組に無い・「…の転換不能」は現象(正準名 不転換)として取れるが「…が不転換」は not_in_vocabulary・「転換不良」は「不良」→ 故障に流れる。D-KB: finding の result は文字どおり「正常」「異常」でしか候補生成しない・症状照合 3 段(部分一致→意味照合→辞書展開・長い語が先)・NFKC は部分一致の症状照合以外。Artifact 版 2 = https://claude.ai/artifact/PGyZijY3yrfLWFNHTwbS5V

**版 3(13:5x)**: 利用者の問い「ask も汎用化されたか」→ 否(汎用化は facets 新設のみ・ask は併記とその記録だけ・応答の形不変・ask 内で facets を呼ぶ統合は無し)をガイド §0/§3-1 と説明冒頭に明記。B-KB 620f352aaaf325a8・説明 330f22743eb02282。Artifact 版 3 と同じ本文を docs/Artifact_B-KB_D-KB_APIガイド_20260928.html(doctype 付き・marked は cdnjs)に置いた。
