---
name: kaitetsu-comments-20260826
description: 海鉄コメント(②36問)対応+F-1読取強化を実施(2026-08-26)。対応表・承認依頼書を作成し、glossary10件/ctx C1/refs再チャンク235件(F系77+R-1〜R-4 158)をステージングで検証済み・本体未投入(承認待ち)
metadata: 
  node_type: memory
  type: project
  originSessionId: cdd0397a-bba7-471e-80ab-cd200525a491
  modified: 2026-08-27T15:04:04.831Z
---

2026-08-26、`docs/指示_208問改善_海鉄コメント対応_20260826.md` に基づき作業。成果物: `docs/eval/海鉄コメント対応表_20260826.md`(36件全件)・`docs/eval/承認依頼_海鉄対応_F1読取強化_20260826.md`・出題確認リストへ追記(海鉄コメント反映+新規(ニ)(ヌ))。

**2026-08-28 3回多数決(同一状態)=原版189(191/188/186)/修正版195・3回一致196/208・不安定12問(半数は出題確認中)。以後の状態比較は3回多数決(majority3.py・EC2 run3.sh)で行う。**

**2026-08-28 direction_lever_ctxを主語判定2段階(必須/参考)へ置換・監査A2(ctx一般化: audit_ctx_generalization.py+言い換え検証セット)新設・合格。**

**2026-08-28 フル測定(D3+打ち手2投入後)**: 貴社提供版のQA判定184(前回187)/修正版190(前回191)・incorrect0。対象5問(013/035/015/067/081)は全てcorrect化、低下8問はctx非発火・骨子不変の判定ゆらぎ(017/030/055は出題確認中)。記録=docs/eval/フル測定_20260827_3点セット.md。EC2測定ジョブはssh切断で死ぬことがある→`setsid nohup sh -c "docker exec …" … & disown`で起動する。

**2026-08-27 打ち手2(013/015/035/067の決定論化+加佐登3L転記是正)も承認投入済み(4問correct化見込み)。**

**2026-08-27 D3対応(限界表示灯T2出典+tk_ctx・時間鎖錠T2 p150)も承認投入済み(043 correct化)。確認事項は24項目(ア〜ノ)。**

**2026-08-26 利用者承認→投入完了**: 本体kg_api/・EC2(6ファイルmd5一致・docker commit済み)へ反映。監査全合格。フル測定=原版187(±0)/保守180+保留7/修正版191(+1)・②36問=27/7/2(低下4問は判定ゆらぎ、030はcorrect化→保留)。コミット済み。以下は投入前の記録。

**状態(投入前)**: 承認事項(G1〜G10 glossary・C1 決定論ctx `signal_control_tc_ctx`・J1 JUDGE_SCOPE 2026-08-26.1・R1 F-1/F-2/F-3+R-1〜R-4 vision転記235チャンク(R系回帰8/8 correct))は scratchpad の `kg_api_stage`(port 8611)で検証済み。**本体 kg_api/・EC2 は未変更**。パッチ/ビルダー/vision抽出スクリプト=`knowledge_kb_v8/scripts/kaitetsu_20260826/`(未コミット)、vision転記JSON+PDF化物=`kb_demo_v6/data/refs_c_vision/`(275ファイル・.gitignore済み)。承認時は承認依頼書§6の手順で本体へ投入(vision再実行は不要)。

**Why**: 統制対象(glossary/スコープ/プロンプト)は人手承認が必須。海鉄J列=顧客自身が示した実出典なので実出典化の好機。

**How to apply**: 承認後は §6 手順→監査→EC2パッチ再生→フル測定3点セット。海鉄ページ番号は当方PDF化の印字ページと不一致(対応表§0参照)。F-1別表1 印字p4はEMFがLOで描画不可→PDF内ラスタ(584×856)+T1 p169活字で照合する。[[eval-improvement-progress]] [[milestone-20260825]] [[legend-extraction]]
