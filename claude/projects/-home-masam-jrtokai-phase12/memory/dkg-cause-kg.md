---
name: dkg-cause-kg
description: "D-KG(原因探索KG) Phase1-5完了(2026-08-25承認済): bolt10090に10,573ノード/16,526エッジ・/v1/dkg/diagnose稼働・機序140投入・SOD付与。作業1-4完了(20260825): Dify対話診断チャットフロー稼働・/v1/federate/dkg新設(既存lite不変)・診断評価100問(D∪B概念@5=0.550/系統@5=0.880)・検知拡充(反証可能原因2倍)"
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-25T00:20:12.257Z
---

「原因探索ナレッジグラフD-KG」Phase1-5完了(全承認取得済み)。計画正本=/home/masam/.claude/plans/fuzzy-exploring-corbato.md、統括レポート=knowledge_kb_v8/reports/dkg_build_summary.md+dkg_phase4_summary.md+dkg_sod_summary.md。

- 構成: Neo4j `neo4j-dkg-v1` bolt **10090**(deploy/docker-compose.dkg.yml)。10,573ノード/16,526エッジ。A-DKB決定論グラフ化+B概念209ミラー+NORMALIZES_TO258+CanonicalSymptom279+証拠エッジ166×2+Mechanism140(T2・span検証済)+SOD付与
- **LOO700総合評価(20260826完了)**: 真のLOO(exclude+頻度減算)で647件実測。系統@5=0.832(実用域)・概念@5=0.425(in-sampleとの差-0.215=リーク実測)・L3実質一致correct1.6%(概念バケット一致≠原文実質一致をスケール実証)・G3断定47.5%。判定器=定義書v1(公平性4種合格・感度は正例注入方式)。正本=reports/dkg_loo700_report.md・docs/判定器定義書_LOO700_L3_v1.md。改善1実施済(example_causes・correct1.6%→11.3%/+partial39.0%・副作用=honest_gap→incorrect89件は低適合時抑制ゲートが次課題)。改善2実施済(ゲートp>=0.4+insufficient3状態化): v3=correct0.143/incorrect0.566/hg回復0.116・G3断定率11.3%。入力ガイド実施済(input_gaps 3点検出+もしかして候補・v10警告+ボタン・Dify v0.3.5冒頭ガイド)。残=人間照合50件(レビューUIのLOO版)
- 診断レビューWebUI(20260826): deploy/dkg-review(Flask・8631・層化50件スナップショット=build_review_items.py生成)。EC2は https://54-250-247-97.nip.io/dkg-review/(Basic認証共通)。4値判定+誤り種別+質問有効性票→answers.json蓄積・/summary集計
- 精度向上(20260826): C1=情報利得質問選択・L1=C系トポロジ併発確認質問(【併発確認】プレフィックス・広域/局所判別)採用。M1スコア較正は@5改善なしで不採用(HB収載順は弱い根拠と知見化・calibrate_score.pyに枠組み保存)
- 絞り込みモード: convergence(決定論収束判定)を応答に付与。Dify「原因探索(絞り込みモード)デモ」(guided=未収束は質問のみ)+v10チェックボックス。質問照合は前方一致20字で頑健化(LLMの末尾省略対策)
- API: `/v1/dkg/diagnose`(kg_api/sources/dkg_router.py・8項目様式・決定論スコア・観測適用×1.5/×0.3・機序/RPN添付・v1非破壊)。接地=kg_api/kb/scripts/dkg_grounding.py(関西線・評価30/30 P=R=1.0)
- 評価: eval_reach_loo.py --study dkg(同値性LOO401不一致0・概念@5=0.022)。タキソノミv0.2.1(map210・保留3)で新基準B∪DKB∪A=0.948
- **重要知見**: ①名寄せ0.60-0.70帯は85%偽陽性 ②SOD付与でD=4(検知手段なし)が83% ③T2機序の概念被覆8.1%=規範と実態の語彙乖離が全層で再現。機序の主価値は説明性

**Why:** B/C正本と分離した実験層で非破壊担保。判定=決定論・LLM=候補提示のみ・確定=人手承認(fan-out判定案→AskUserQuestion承認→confirm列→投入、のパターンが確立)。

**How to apply:** 残課題3件完了(20260825後半): 観測→B伝播(発火確認・応答全体概念@5=0.640/系統@5=0.930)・機序名寄せ+18(被覆22)・検知直付けDETECTED_BY59でD=4を66%へ削減。改善サイクル2完了: 機序T1/HB拡大(Mechanism319・被覆31/209)・hold22確認(y3・残2)・S補完=元記録欠如で不能と確定(拡充には顧客の運行影響データ提供か定性S尺度の承認が必要)。Difyプラグイン更新はv0.3.0手順(作業記録§9・console APIはBearer=access_token Cookie+X-CSRF-Token)。ローカルAWS認証はsaml2aws login(利用者が! で実行)。EC2のkg_apiバインドは127.0.0.1でなくプライベートIP:8600。EC2ネットワークはjrtokai-v9_default(deploy_jrtokai-netではkg_apiからDNS不可)。関連 [[milestone-20260825]] [[v8-cross-source-federation]] [[eval-and-table-comprehension]]

- mode=normative追加(2026-09-01・コミット済・EC2 norm-20260901): 依拠文書のみモード(B実績不使用・
  手順トレースprocedure_trace主役・step観測で分岐実行・異常時はon_abnormalを候補化)。
  LOO correct 4.2%=正答はB実績支配の再確認→役割は手順ナビ・根拠提示。coverage(説明被覆
  タイブレーク)+2段対偶は両モード共通で非劣化。観測タイプ=question/measurement/finding/step の4種
