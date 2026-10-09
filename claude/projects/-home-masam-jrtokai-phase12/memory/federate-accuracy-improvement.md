---
name: federate-accuracy-improvement
description: federate精度向上=完了(2026-09-11)。精度はホールドアウトで同等・時間−25%・出典正確性が確定効果。案A/案D/no_infoは無効と実測(no_infoは撤去)。決定論違反(ハッシュ乱択)を是正しEC2反映済。監査X1-X8+X1-2/X3r/X6b/X15をaudit_fed.pyで機械化、正本=dfix2
metadata: 
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-08T20:40:15.539Z
---

2026-09-09、検討書 `docs/検討_federate精度向上_20260909.md` を作成し利用者承認。P1(same_place b-0)・P2(b1_query 半角カナ+固有名^3)・P3(base_rates/lift, decide_no_info θ=b1<8∧同一箇所なし∧lift<3, question_block ⑤)・P4(C駅スコープ+選択起動+extra_terms=cluesのみ)を federation.py に実装済。B根拠上限8→26(候補が2件しか渡らない既存不具合)。P5/P7(FUSE_SYS_TAG・expand_citation_tags・FED_CITE_TAGS=1)は実装済だが既定オフ=プロンプト差分の個別承認待ち。再評価スナップショット=fvd_*_p14*.jsonl(git外)、判定・集計=scratchpad judge_report.py(J1 sonnet/J2 gpt-5-6-sol via kb_demo/J3 j3_det.py)。

実測で確定した事実(50問比較評価の再分析):
- 所要時間の90%超は合成LLM(federate=opus-5/4000tok≒38s、lite=sonnet-4-6/2000tok≒24s)。検索はA rerank 0.6s/回・B 0.1s・C 0.1s。
- 出典文字列が回答の36%を占める。lite は 30/50 が max_tokens 打ち切り(④途中で切断)。
- b-3a は 42/50 で手掛かりが一般語→MAX_ACC=80 到達=基礎率候補(BU受信器 基礎率9.7%が1位 27/50)。
- C は質問文のみでは 2/50 しか接地せず、B候補原因文の番号混入で伊那北/伊那市31号・転てつ器53へ誤接地 7/50(回答混入13/50)。
- 全文索引は文字単位標準アナライザ。全角カナ質問語は半角カナ原文に不一致(モリブデン0件/ﾓﾘﾌﾞﾃﾞﾝ17件)。
- 絞り込み質問(meta.question/flow_questions)は合成に未使用。

改善案 P1同一箇所直接照合(AT_STATION/AT_CROSSING/IN_SECTION)・P2クエリ正規化+固有名ブースト・P3 lift+no_info決定論判定+絞り込み質問・P4 C誤接地防止+選択起動・P5出典タグ化(プロンプト変更・要個別承認)・P6モデル統一実測・P7質問文観測事実の扱い。

**Why:** federate側の改善はD-KB側セッション(検討_DKB再発照合パス)と独立に進めるが、評価枠組み(データ3分割・J1/J2/J3・監査パッケージ)は共通。ホールドアウト50問(乱数種1)はD-KB側が `fvd_holdout_sample.json` として抽選(2026-09-09時点未抽選)。
**How to apply:** 承認後は P1→P2→P4→P3→P5/P7→P6 の順で実装し、`run_fed_vs_dkb.py` は編集せず suffix 付き `run_method` で呼ぶ。no_info 閾値はホールドアウト抽選前に固定。関連: [[fed-vs-dkb-comparison]] [[federate-latency-optimization]] [[dkg-cause-kg]]

追記(2026-09-09 09:00): P5/P7 承認→既定有効(FED_CITE_TAGS=1)。退行是正=同一箇所の並び(踏切>駅>区間→症状語一致→日付)+症状語一致注記。**LOO除外漏れ(双子=重複取込194/700・開発11問/ホールドアウト18問)を発見**→`accident_twins.json`(git外)で除外集合を拡張、基準版はHEAD作業ツリー(scratchpad/base)で再実行(fvd_federate_tw/lite_tw)。D-KB側へ `docs/連絡_LOO双子除外_DKB側セッションへ_20260909.md`。既存50問(双子除外・同一判定セットS3・J1): 基準56%→P5 62%、incorrect 17→14、悪化0。sonnet統一(P6)はJ1で−8pt→採用不可の見込み。ホールドアウト= fvd_{federate,lite,federate_s}_p5_h.jsonl 実行中。集計= scratchpad/judge_report.py report _p5 / _p5_h。J1のゆらぎ±3問(6pt)を実測。

追記(2026-09-09 11:00): ホールドアウト(双子除外・同一セットS7)= 改善前federate J1 65%/J2 83% → 最終P1-P5 J1 63%/J2 83% → **c+p改善は確認できず**(既存50問の+6ptは一般化せず)。確定効果=所要時間 federate −27%/lite −19%、出典自己訂正0、C誤接地0、lite打ち切り減、双子除外による評価妥当性。副作用2件をP8で是正(b-1固有名ブースト廃止=b-0が箇所再発を担保・一般語注記の文言を優先順位指示に限定)→ fvd_*_p8_h 再実行中、判定S9(アンカー=federate_tw_h)。レポート= knowledge_kb_v8/reports/federate_improvement_eval.md。D-KB側は改善主張を撤回済(dc4ec0e/9f166c7)。未コミット(federation.py/federate_backend.py/docs/reports)。

最終(2026-09-09 11:45): P8是正後のホールドアウト(S9/S10)=精度同等(federate J1 63→63/J2 86→86、lite J1 62→64/J2 79→81)・悪化0。確定効果=時間(federate −25%/lite −17%)・出典正確性・C誤接地0・lite完走46/50・双子除外。P6(sonnet統一)不採用。状態=コミット承認待ち(federation.py/federate_backend.py/検討書/連絡文2/レポート)・EC2未反映。基準版worktreeは削除済。

完了(2026-09-09 12:00): 利用者承認によりコミット(federation.py/federate_backend.py/検討書/連絡文2/EC2依頼文/レポート)。lite max_tokens 2500→3000(利用者判断・再評価未実施)。EC2反映はD-KB側セッションが同時実施(依頼文 docs/依頼_EC2反映_federate改善_DKB側セッションへ_20260909.md)。残課題=lite到達問の再評価・初見層(類似0件)・B重複取込194件の統合(人手承認・D-KB側提起)。

Phase3完了(2026-09-09 18:00): fed側 Phase3(4ed1356)+engine正本化(dkb 2f9fd87)の非劣化をホールドアウト同一セットS12で確認(J1 67→69%/J2 87→87%/J3同値/リーク0)。EC2反映は coord へ req 済(federation.py fb2ad5c/4ed1356 + engine 2f9fd87)。評価LOO除外は SourceB.expand_exclude(DUPLICATE_OF由来)で自動化。スナップショット fvd_federate_ph3_h / ph3e_h(git外)。

lite系統ランク再ランク(2026-09-09 19:30): EC2(CPU)で lite の a_system_ranking 再ランク(POOL30)が約64秒。案A(省略)は系統@3 70→57%/52→32%に低下、案B(候補10)も22/16で案A並み(上位10件集約のため構造的)。候補20なら25/21でほぼ維持(EC2 −約20秒)。LLM段はいずれも非劣化相当。実装=FED_SYSRANK_FOLLOW_LITE / FED_SYSRANK_POOL(既定は従来・fed cb56f1a)。利用者判断待ち(rep needs_user_approval)。

決定(2026-09-09 19:30 dec 1930): lite 系統ランク再ランク候補数=20 を採用。EC2 反映済(coord・保全タグ pool20-20260909・compose env FED_SYSRANK_POOL=20 は EC2 側のみ、ローカル正本 deploy/docker-compose.v9abc.aws.yml への追随は利用者判断)。fed 側作業完了(78dc19c)。

中断(2026-09-10 14:00・システム再起動): 再開手順は docs/comms の info(pause-state)に記載。作業ツリー=/home/masam/jrtokai-phase12-fed(ブランチ fed)・稼働プロセスなし・git クリーン(634007d)。完了=fvd_federate_{a20,a15,fs1}_h.jsonl 各50問。未了=案D(lr1)実行・S16/S17 判定・rep 3件・監査 X1-X8 本実行・X1 残20件の内訳 info。評価スクリプトは knowledge_kb_v8/scripts/fed/{fed_eval.py,audit_fed.py}(リポジトリ内・scratchpad は再起動で消えるため使わない)。実装済み env: FED_A_POOL / FED_FIRSTSEEN_ROUTE / FED_FIRSTSEEN_LIFT / FED_LIFT_RANK / FED_LIFT_MIN_ACC / FED_LITE_MAX_TOKENS(いずれも既定=従来動作)。

完了(2026-09-11 06:00): 監査 X1〜X8 を `knowledge_kb_v8/scripts/fed/audit_fed.py` で機械化(PYTHONHASHSEED=0 で自己再実行・FED_AUDIT_CONF_DIR で辞書差し替え検証可)。fed 担当の要是正 0 件、残る WARN は X6b(意味照合の閾値マージン 19%・恒久対処は req 起票済で保留)。確定事項:
- **判定器のノイズ**: 同一回答が別バッチで最大 28% 変わる(同一バッチ内の複製は 0/50)。比較は必ず同一バッチ、単独バッチの絶対値は使わない(dfix 68/70%、dfix2 76/70% の実例)。
- **決定論違反**: 共有 accident_diag_engine.py の同点並びが set/dict 反復順依存→ハッシュ種で候補集合が 11/50 問変化。同点キー固定で 50/50 一致(X3r)。`os.environ.setdefault("PYTHONHASHSEED")` は起動後なので無効。EC2 反映済(dfix-20260910)+compose に PYTHONHASHSEED=0。
- **X15 の型**: 案A・案D・手掛かり4件・X8 順位変更はいずれも「較正/全体分布で見えた相関がホールドアウト/LOO で消える」。相関由来の改修は既定OFF+実測注記で残す。
- decide_no_info は撤去(発火 2%・条件 2/3 が逆相関)。案D は表示方式(FED_SPECIFIC_MARK・各問1件の印+読み下し)として実装、EC2 で有効。
- 正本スナップショット = fvd_federate_dfix2_h.jsonl(判定 S22 同一バッチで dfix と差 0)。
- 辞書の固有名混入は dkb と共通定義「basis に共起を含み地名を含む別名」で 27 件投入済→X1-2 0 件。B 正本の踏切名接尾脱落(旅客通路系 4 ノード)は 3 段是正案が利用者判断中・SAME_PLACE_AS 投入時に same_place 側の追随要否を確認する約束。

追記(2026-09-11 08:00・X16 手順 6): 新文書投入のオーケストレータ `knowledge_kb_v8/scripts/fed/ingest_docs.py`(段ゲート G0〜G8・pipeline_state・resume・rehearse・開始時ダイジェスト検査)を実装、隔離演習で完走。**接続先の既定値はコードから撤去済(X13・kb_conn)**: 評価・監査・オーケストレータを動かすには `KB_ACCIDENT_NEO4J_URI=bolt://localhost:9890 KG_DKG_URI=bolt://localhost:10090 KB_NEO4J_URI=bolt://localhost:9990 KG_NEO4J_PW=<ローカル開発用>` をシェルで設定する(値はリポジトリに書かない)。`kg_api/app.py` は `PYTHONHASHSEED=0` 未設定で起動拒否(fail-fast)。集約ドキュメント = `docs/federate_再構築手順_統括.md`(各 § に最終更新日・監査 X16 が実装との乖離を検出)。llm_providers は `kg_api/kb/scripts/` が正本(dkb 一本化・kb_demo_v6 は相対パス再エクスポートに是正済・集約ドキュメント §4.4/§10 #12 反映済)。

確定(2026-09-12・X16 手順 6 合格): 両系列で「手順のみ」0 件(fed 7→0・dkb 19→0)。**唯一の投入経路** = fed `knowledge_kb_v8/scripts/fed/ingest_docs.py`(G0〜G8)/ dkb `accident_kb_v7/scripts/ingest_pipeline.py --mode commit`(G0〜G11)。以後の運用: 四半期と新種データ到来時の隔離演習(`--rehearse`)、定期監査(`run_audit_periodic.sh`・cron 登録は利用者判断待ち→手動)、coord の週次報告。fed 残件 = X6b(保留)・共有ファイル(config.py/neo4j_client.py)の kb_conn 移行(所有確認後・暫定許容表)・#4b 索引の世代バックアップ。

追記(2026-09-11 夜・fed a23171e): ckb 共有 app.py レビュー(判定スコープ外出し・scope_sha・cachePoint・× 整形・prompt_store 起動時検査 verify)は全て合格・ack 済、coord が EC2 第 1 弾(wave1-20260914)反映中。fed 宛 open なし。fed→coord open は req 2251(X6b 恒久対処・急ぎでない)のみ。main への取り込みは coord が fed ブランチを merge する運用。

追記(2026-09-11 夜・次の打ち手 rep 2043・req 2100): 新規計測=gold 概念が B 証拠(b-3a 上位6/同一箇所/b-1 事故の概念)に到達した問 24/50 は c+p 23/24(96%)、未到達 26/50 は 12/26(46%)、失敗 15 問中 14 が未到達=到達性が支配要因。b-3a 候補の名指し率 1 位 45/50→6 位 5/50(gold 5-6 位の 8 問中 partial 6)。b-3a 候補数はエンジンで 6 固定(dkb 所有)。A 取得と系統ランクの再ランク候補は 88% 重複(EC2 130 s のうち CPU 再ランク約 100 s)→ スコア決定論キャッシュで −35〜40% 見込み。X6b の意味照合(DKB_SEM_THR)は federate 本番経路外(dkg_backend thr 0.45 と対話診断のみ)。正本 dfix2 の同一バッチ値は S21 76%/S22 70%(「72%」は P1-P5 版の統一バッチ)。推奨=案 1 評価二段化(到達率主指標+J1×3 多数決)+案 2 再ランクキャッシュ。rev 統合版は coord 転送待ち。

訂正(2026-09-11 夜・統合版 rep 2051・rev R3/R5 で確定): (1) 案A 初見層ルーティング(dec 1315)の「効果なし」は誤り=A ブロック上限 8 件(_ev_block limit=8)で規範候補が LLM に 0 件しか届いていなかった(発火 12 問すべて)。現行本番でも系統候補 3 件が 33/50 問で 1〜2 件に切詰め。「未検証」扱い。(2) 案D 印(FED_SPECIFIC_MARK)は候補本文の読み下しを変え合成入力に入る=表示のみではない。正本 dfix2 は OFF で生成・EC2 は ON、ON の非劣化は同一バッチ未判定。統合後の推奨: 順 1 到達記録と評価の整備(費用 0)・順 2 再ランク決定論キャッシュ(承認不要)・順 3 是正検証 2 件(印 ON 非劣化・A ブロック配分)。多数決は判定条件の変更として別承認。利用者承認待ち。

第 1 期完了(2026-09-11 夜・dec 0010): 順 1=到達記録(meta.fuse_input/evidence・出力不変)+report の分母/遷移表/TOLERANCE(c+p 低下≤1・inc 増≤1・改善主張 +2 かつ J2)/指標改称/到達性 §5。順 2=再ランクスコア決定論キャッシュ(retriever・プロセス内 query 単位 LRU 256・受入 50 問完全一致・EC2 full 120→83 s・保全 rerank-cache-20260915)。順 3(同一バッチ S23・J1): 案D 印 ON=非劣化確認(70%=70%・不変 49)、FED_A_BLOCK_ALLOC(既定オフ・A 種類別枠 8/3/3)=効果なし(68%・不変 49)。新発見: B 上限 26 でも同一箇所が多い問 2/50 で診断候補 5〜6 位が落ちる。判断待ち: EC2 印 ON 維持・alloc 既定オフ・順 4(案 3 b-1 窓/案 8 b-3a 上位 N を到達率で先に確認)。判定タグ w1・バックアップ世代 20260911-2149。

順 4 中間(2026-09-11 22:00・dec 0430): X15 台帳の案D 印=S23 判定済へ、TOLERANCE を監査付録 F 系に正式記載。B ブロック『診断候補を落とさない枠』FED_B_KEEP_DIAG 既定 ON 実装(入力が変わる問 7/50・EC2 反映 req 済 federation.py d3828a3a)。案 3 b-1 拡張窓(FED_B1_WINDOW/FED_B1_EXT_MINHIT 既定オフ)は到達 +1 のみ(W=20/一致≥1)で生成見送りを提案。案 8 は dkb へ req 2201(candidates_all 上位 15 加法)→ 対応後に N=6/10/15 の到達率計測。待ち: coord(案 3 見送り確認・EC2 反映)・dkb(案 8 I/F)。

第 1 期完了(2026-09-12 04:18・rep 0418): 順 4 S24(J1・dfix2/keep1/cand1): keep1(B 枠 FED_B_KEEP_DIAG 既定 ON)=72%=72%・50/50 不変=非劣化確認→EC2 反映は利用者判断(federation.py d4c6dff0)。cand1(候補表 FED_CAND_TABLE)=不変 49・+1 correct=改善なし→既定オフ維持。案 3(b-1 窓)+1・案 8(上位 N)+0=記録のみ。精度は 72% で同等、確定改善=再ランクキャッシュ・到達記録・B 枠。未到達 26/50 のうち 20 問は語彙側の構造的限界。次期は案 5 較正表示(誤誘導低減)と時間・説明性へ軸足移行を提案予定。判定タグ w2・バックアップ世代 20260912-0417。llm_part は ⑤ と候補表を除外。

第 2 期(2026-09-12・dec 1030): B-1 options.a_rerank(None=従来)、B-2 2 段 API /v1/federate/retrieve→/synthesize(federation._synthesize/synthesize_from・一括と byte 同一・test_federate_two_stage 10/10)→EC2 反映 req 1020(4 ファイル)。C demo_v10 に『根拠の構造』欄+2 段選択(加法・所管未登録→fed 提案)、Dify は answer のみ(coord 判断)。D 未到達 26 内訳を dkb へ info(概念未登録 10・支持 0 が 10・双子のみ 4・窓外 2)。B 枠は EC2 反映済(bkeep-20260915)。案 5 較正表示は rev 確認(req 1000)後に設計 rep→生成・判定($7)。監査レポートは日付ファイル audit_fed_20260912.md。

rev 第 2 期確認(2026-09-12 11:28・rep 1128×2): R1〜R3 全採用。追補 c15487b=2 段合成は k/lite/provider を retrieval 継承・契約明文化(a_rerank は A 取得のみ)・C 取得失敗は meta.C.error・UI は 404 のみ退避・候補列は取得同定集合+到達件数。S24 の未到達→c+p は 13/26(0418 の 12/26 は S22 値)。案 5 設計: 第 1 案=synthesis.calibration(B 手掛かりの強さ・閾値 0.15・版 calib-2026-09-12.1)を API 加法+UI 欄(費用 0・誤誘導低減は主張しない)、第 2 案=回答先頭付記を keep1 保存回答との後処理対で J1 約 $2。J1 は回答先頭 1,800 字のみ判定。承認待ち。EC2 反映 req(5 ファイル)提出済。

案 5 第 1 案 実装(2026-09-12 13:20・dec 1900・fed 179f3ca): federation.calibration()=B 手掛かりの強さ(meta.B.confidence <0.15 低/<0.40 中/高/未評価・厳密 <・版 calib-2026-09-12.1・コード定数)→ meta.calibration・API synthesis.calibration・demo_v10 バッジ。回答文/合成入力/判定条件は不変(テスト 44/44・2 段 13/13・UI PASS)。診断用集計 S24: 低 13(correct 0・partial 6・no_info 2・incorrect 5)/中 28/高 9。第 2 案・独立確認群は見送り。EC2 反映 req 1320 提出→反映後に契機 (3) 監査。

第 2 期完了(2026-09-12 13:30): A 案 5 第 1 案 EC2 反映済(calib-20260915・fused 2 問 fuse_input/audit 不変・calibration 付与)。契機 (3) 監査 要是正 0・WARN 1。第 2 期 A〜D 完了。残件: X6b 保留(req 2251・federate 経路外)、独立確認群 未確保(一般化未確認)、Dify 構造化欄 見送り、案 3/4/8 は記録のみ(実装は既定オフで保持)。監査契機の運用(手動・5 契機)は継続。

X6b 保留継続(2026-09-13・dec 20260916-0500・fed 515e459): 順位・相対マージン方式は着手しない。WARN は既知件として継続記録(合格基準不変)・運用担保=比較を同一環境に限る。理由: 意味照合 dkb_semantic_symptoms は federate 本番経路外(対話診断=未使用・D-KG 症状提案のみ)/順位方式でも境界逆転は残る/較正やり直しは D-KB に波及。再検討の契機=D-KB の症状照合改修 or 実行環境変更(GPU→CPU)→ fed・dkb 合同で較正・新規 req。req 2251 closed。監査コード X6b・集約 §6/§10 #13・監査付録 F 系に記録。連絡箱 open 0 件。
