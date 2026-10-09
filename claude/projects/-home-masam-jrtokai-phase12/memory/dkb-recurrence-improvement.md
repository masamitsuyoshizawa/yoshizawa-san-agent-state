---
name: dkb-recurrence-improvement
description: 双子除外の統一比較確定(fed72≈lite70>dkg52-56)・D-KB P1-P4=correct化のみ・採否とB重複統合が利用者判断待ち
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-09T01:51:30.589Z
---

2026-09-09完結(eaf19f5)。経緯: P1-P4実装(2b28e82)→双子(重複取込194/700件)発覚→除外拡張
(loo_exclude/twin_mention)→c+p改善撤回(dc4ec0e)→6方式統一比較(双子除外・同一バッチブラインド
J1/J2/J3)で確定。
最終結果(ホールドアウト50問・J1): federate P1-P5 72%≈lite 70%>D-KB 52-56%。
- D-KB P1-P4: c+p改善なし・correct化+5問(3→8)・ノイズ降格。
- federate P1-P5: 精度維持+2ptで時間-27%(42.6→31.0s)。lite P1-P5は悪化傾向(J3-6pt)をfederate側へ連絡済。
- 判定器教訓: 提示案数で絶対値が大きく動く(J2は3案64-84%→6案34-50%)。**絶対値は同一バッチ内比較限定・
  主張は複数判定器の方向一致で**。κ=0.55。
レポート=knowledge_kb_v8/reports/fed_vs_dkb_final_unified.md(sha256監査)。
2026-09-09両方前進: ①EC2一括反映**完了**(federate P1-P5+P8とD-KB P1-P4・保全タグunified-20260909=federate-p8-20260909+latest・スモーク全合格・dkg_backendのB接続をKB_ACCIDENT_NEO4J_URI env化=既存2箇所の潜在バグも解消d2d207b・APIバインドは172.31.4.27:8600でlocalhost不可) ②B重複統合=全承認・Phase1完了(a05e893): DUPLICATE_OF 98本をローカル+EC2投入(相互0/連鎖0)・発生日検証で1ペア(acc:0556/0321=要旨誤付与疑い)除外・顧客確認シート98+1件=knowledge_kb_v8/data/eval/顧客確認_B重複取込一覧_20260909.xlsx(git外)。顧客確認完了(9/9): 98組同一事故で確定・acc:0556=要旨誤付与で除外確定(excludedフラグ両環境+dkg_backend4クエリへNOT coalesce(a.excluded,false)フィルタ・EC2保全excl0556-20260909=latest・acc:0321は有効維持)。Phase2/3完了(6a2c4d7・両環境phase23-20260909=latest): D-KG頻度正本単位化(41概念・確定330→285・certainty最良採用・旧値freq_*_predup保全・recount_freq_canonical.py)+再発照合/実例引用の正本集約(出典「acc:0306(=acc:0500)」併記・原因はグループ全ファイルconfirmed優先・LOOは正本連動除外)。非劣化=J1変化0/J3同値/リーク0。残=diag_engine計数(規則提案済・実装者をfederate側と調整中)・SourceB系折りたたみ+excludedフィルタはfederate側分担。利用者確認済み解釈: 重複=同一事故の続報つき別ファイル(続報58/再掲40組)・Phase3提示はグループ全ファイル統合(初回のみ表示は続報情報が漏れるためNG)。重複の状況と扱いは総括§2.4/カルテ注記(実質約600事故)/顧客pptxスライド2-2/Artifact2本へ記載済(efee1a1)。確認シートは初回/続報表記へ改訂済。
関連: [[fed-vs-dkb-comparison]] [[federate-accuracy-improvement]] [[dkg-cause-kg]]

LOO700全量再実施(20260909・tw20260909・req 1606): c+p 47.3%(c134/p134/hg16/ic279/pe3・566問)・9/4正本46.2%比+1.1pt・correct+22。悪化59中21問=双子リーク剥落の正常化を吸収しての微増=実力の伸びは見かけ以上。正本単位(群内最良488群)47.7%・G1 52.5%/G2 35.1%・リーク0。スナップ=responses_tw20260909(注: _v4等は8/26旧試行と衝突するため日付サフィックス必須)。engine計数正本化はfedレビュー合格(top1同一92/100)・EC2反映済(coord一括・保全phase3-20260909=latest・スモーク合格)。A案dkb分担は全件クローズ。

正答率向上P-A〜P-D(req 1926・f5bd58d/f536f41・20260909): P-A系統ゲート(辞書system+文脈語→Family・別系統×0.3/その他×0.5)+P-B概況症状句B照合(kind=b_text・線区語幹・federate b-1の概況版)+P-C実例昇格(b_sim)+P-D地名復元(文脈照合一意のみ・誤復元例「城[氏名]→城前」を辞書一意方式で検出し文脈方式へ変更・旧値保全)+CONTAINS279本。効果(較正込み最終): LOO700全量 c+p 47.3→50.2→**51.4%**(correct 134→182・ic 279→263・hg16→9→12=誠実性較正req 2035でb_text固有性階層化df<=10単独可/カナ音写除外/b_sim n_acc>=3・悪化50問中25回復)・ホールドアウトJ1 55→73/J2 52→60/J3 26→34・fed専勝10問中6回収・liteを逆転。悪化50問(p→ic24・hg16→9=誠実性副作用)は残課題。EC2側P-D+P-A〜Cはcoordへreq済。

総合監査+fed知見適用(20260910・req 1330・fd6c3b3): 監査スクリプト knowledge_kb_v8/scripts/dkg/audit_dkb.py(X1〜X10・読み取り専用・--loo700/--verify)を実装。初回=PASS7/WARN2/FAIL0。**重要な実測**: top1候補の種別別 c+p = direct82/dict_eq76/b_sim69/recurrence62/b_text50/**HB規範29**/候補なし0(LOO700 470問)。D-KBの弱点は基礎率支配でなく『規範候補しか出せない問17%の正答率29%』。並べ替え余地は99問(19%)あるが同一種別内29問で係数較正では救えず、症状接続数(中央値2)は誤答/正答で差なし→ fedのlift(F1/F2/F4)はD-KBでは効果限定的・既定変更せず(DKB_DICTEQ_BASE/DKB_BTEXT_STRONGは既定=従来動作)。F3逆相関0件・F7指標不一致0(該当なし)。F5=P3/P4がほぼ死条件(2.1%)・『断定できない理由』は常時発火で誠実性指標に使えない。

P-E語彙拡充(req 1411(b)・dec 1522承認・20260910・cda955f): 未正規化Cause 364件を(設備語,述語)クラスタ→新概念12+既存概念へのリンク76行+**Family『外的要因』新設**を加法投入(未正規化364→288・NORMALIZED_AS 1:1)。効果=LOO700同一566問で全体 c+p 51.4→**53.0%**・ic 263→249・**G2層 38.7→44.0%(+5.3pt)**・G1不変。b_text再較正(req 1411(a))は6構成を掃引し現行が最良で既定変更なし(厳格化はhit@1-10/hit@5-30)。投入の型: 計画JSON(決定論規則+expected_support)→apply_vocab_pe.py --dry→投入時に支持件数を再検証し不一致なら中止=二重投入防止。scripts: propose_vocab_pe.py/apply_vocab_pe.py/audit_dkb.py。

**実行環境差(20260910・確定)**: federation.dkb_match_symptoms の意味照合(埋め込み・DKB_SEM_THR=0.60)はLOO700 566問中495問(87%)で発動し、閾値からの余裕は中央値0.0298・余裕<0.01が17%と密集。GPU/CPU差で採否が反転しEC2とローカルで候補数が1件ずれる(反映漏れではない)。対処=コード変更せず運用(**評価の絶対値比較は同一環境に限る**)。監査に恒久チェックX6bを追加(余裕分布・10%超WARN)。レポートのLOO700値はローカル実行である旨を注記済。

**Why:** 双子除外後の序列と各改善の実効が確定。今後の全LOO評価の標準条件。
**How to apply:** LOOは必ずloo_exclude(双子除外)。判定値の引用は同一バッチ内のみ。
PYTHONHASHSEED=0を先に設定(import時execvでヒアドキュメントが無言死)。
