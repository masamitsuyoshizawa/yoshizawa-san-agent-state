---
name: dkb-coverage-facets-20260925
description: 決80(2026-09-25)D-KB のカバレッジ拡大の検討 — 194/250/67 は現象→症状ではない・現象→症状の被覆 113/891・症状から候補を作れない問 119/646・症状 3 件の上限は意味照合を抑えている
metadata:
  type: project
---

2026-09-25、利用者指示(決80)で D-KB(/v1/dkg/diagnose)の検索カバレッジを facets の同定と 3 切り口で広げる検討。dkb の分は rep 20260925-1648(実装は承認の後)。

- **訂正**: NFKC 同定 194/250/67 は語彙 DB の現象の主張の設備の欄 511 件 → 設備の解決(決49)で、現象 → D-KG 症状の対応ではない。現象 → 症状の対応表は既存に無い。
- **被覆(確定)**: 現象 891 のうち D-KB の症状に橋渡し 113(12.7%)・部分一致の規則で 312。surface_map の symptom 欄 43 種は D-KB 索引にはあるが D-KG の Symptom 名は 8 種。
- **現状値(基準値 jv2_20260921 の応答・git 437b599)**: 症状 0 件 53・症状はあるが D-KG の原因つき症状なし 66 → 計 119/646。照合された症状の 30.2% は D-KG に無い名前(索引の正準 102 が D-KG に無い)。
- **上限**: 症状 3 件は部分一致には 16 問しか効かず、意味照合(bge-m3・0.60)のゆるい一致を抑えている(外すと 383 問が 3 件超・最大 394)。設備 2 件 × 故障様式 2 件は 08-27 Phase D の承認規則(score 0.8+min(0.4,b_freq/20))。
- **不変の確かめの型**: verify_safety_mark / verify_flags_4combo / verify_method_field(LOO 646・別プロセスで旗・欄を除いた sha 一致)。
- **判定器 v2 の 1 回**: 入力 350,903・出力 37,316 トークン(Sonnet 4.6・定価換算 約 1.6 ドル・推定)。
- 推し: 案 B は上限を外さず、facets の同定を別経路で足し組ごとに上限。D-KG に無い症状名 102 の寄せは安い改善(統制語彙の変更 → 承認)。

関連: [[v3-vocab-db-w1-w4-done]]・[[loo700-baseline-judge-v2]]・[[asymmetric-standard-for-gain-and-loss]]
- **決81(2026-09-25 16:57・利用者)**: 段 1 = 案 C + 案 A → 段 2 = 案 D → 段 3 = 案 B。dkb の段 1 計画 `docs/計画_D-KB段1_特異点と語彙DB_dkb_20260925.md` 85651bd0(コミット f5df9a36・rep 1703)。**facets の get_vocab の equipment_entries は別名を方針で絞り basis/level を落とす → 案 A は fed に「絞る前の生の辞書の口」が要る**・DB 経路の別名の並びは JSON と違う → A-0 で並べ替え 3 通りを測る。alert は order 0。fed に口 3 つ・bkb に CauseConcept の経路を req 1703。案 D: 102 はすべて symptom_lexicon(統制)由来・包含 20・残り 82 は人の判断。
- **決81 追補 2(17:04)**: alert の文の型(強・中の固定文言・値は {} だけ)・unavailable は診断を止めない。計画 第 2 版 b66d9a0d(コミット 6e0dfda2・info 1705)。段 1 の実装承認は fed・bkb の計画がそろってからまとめて諮る。
- **fed の口 3 つ OK(rep 20260925-1705・fed 計画 db4fbae3 §2-2)**: equipment_source(vocab)(JSON 経路は dict_equipment.json の中身・深い写し)・search(facet_keys)・search(exclude_ids)(照会の中で除く)。4 つ目の切り口 key = same_equipment_any。dkb 計画 第 3 版 7ec412df(コミット 0ba6b4b2)。bkb の CauseConcept の経路待ち。
