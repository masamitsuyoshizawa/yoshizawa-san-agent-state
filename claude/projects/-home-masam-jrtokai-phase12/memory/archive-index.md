---
name: archive-index
description: MEMORY.md から移した閉じた項目の索引(図表 KB の個別是正・V3 第 1〜2 段・D-KB カバレッジ・順の明示など)。過去の是正や受入の経緯を探すときに読む
metadata:
  type: reference
---

MEMORY.md が読み込みの上限に近づいたので 2026-11-07 に移した。行は元のまま。今も効く規則や現行の状態は MEMORY.md に残した。

- [V8 cross-source federation](v8-cross-source-federation.md) — A/B/C横断統合が完成・出典検証済み(事故ID裏付け1.00)
- [Added data integration 2026-07](added-data-integration-2026-07.md) — C(関西線連動16+踏切94)を9990へ投入
- [Reminder: 踏切制御表ルール化](reminder-crossing-control-table-rules.md) — 鳴動条件の意味体系ルール化=完了(鳴動側20260816+終止側20260817
- [文字飾りタグ移行](decoration-tag-migration.md) — 連動図表の○◎□をcamelCaseタグ<circle>/<doubleCircle>/<rectangle>へ移行済
- [踏切図 進行定位抽出](crossing-diagram-jiso-extraction.md) — 進行定位を幾何(輪郭太さ)で決定論判定・全94本の踏切図データ
- [図表KB評価改善](eval-improvement-progress.md) — 最新(2026-08-25): 原版187(89.9%)/保守181+保留6/修正版190(91.3%)・incorrect0
- [踏切TCトポロジー](crossing-tc-topology.md) — 全94図から軌道回路の配線(所属/隣接NEXT_TC/境界信号)をvision+決定論スティッチでグラフ化
- [区間付与と028/034](section-and-028-034.md) — 駅間(区間)を決定論付与しinterlocking-034(上り1=5/下り1=3現示)正答化、028(イ
- [凡例抽出](legend-extraction.md) — libreoffice導入→実マニュアル(F-1/R-2)の図記号凡例をvision抽出しglossary化
- [信号記号認識](signal-symbol-recognition.md) — 主たる=最右最左/進行定位=輪郭太さ(a)・懸垂通常=密クロップvision二択(b)を四日市で確定し041正答化
- [現示/時素の幾何修正](aspect-jiso-geometry-fix.md) — 斜線独立判定(line45=注意/line135=進行)+時素の左右差判定
- [鳴動条件の決定論読み下し](crossing-sounding-readdown.md) — 図表KB鳴動条件を停止/終止から決定論読み下し・回答層へ加法注入(041正答化・019/017/035は正答相当)
- [図設備記号2.1a是正](symbol-legend-2-1a.md) — 括弧記号(XX)は形状依存判別(遠方/中継/列車停止標識)、対向数は決定論計数、線の太さ/バックアップ地上子の凡例
- [×連動表と出題誤り](clearance-x-boundary-turnout.md) — ×の該当連動表は質問TCの全条件を提示+定/反を幾何推定(確度明示)。×8問incorrect3→0。出題誤り4件
- [事故摘録QA状況](accident-qa-status.md) — B系4問=3問correct相当・Q1残=事故情報記録簿(構造化DB)未統合
- [KB品質是正20260814](kb-quality-sweep-20260814.md) — 未確定フラグ全量是正(補助キロ23/種別23/現示36/個別3+利用者確認で残全解消)・現示線種較正規則
- [TC命名規約の決定論同定](tc-naming-rules.md) — Phase10=T2実出典の命名規約で所属自動解決・同定保留120→0・トレースロスト35
- [現場扱いてこ記号ルール](genba-lever-symbol-rules.md) — 図読取ルール確立・全16駅12記号・既知リスト是正(サ転てつ器/162黒半円/NO2)・承認投入済
- [切り分け経過欄提案](isolation-field-proposal.md) — 様式設計+遡及構造化596件・9/16 v2 再測: 判別できるのは②悪化のみ・④有意は σ 2倍誤りで撤回(z1.47)
- [指令指示支援KB](shirei-instruction-kb.md) — P1-P4完了(通知837/機材151/措置371/規制392)・actionsフルセット化・P5不実施
- [federate精度向上検討](federate-accuracy-improvement.md) — 完了(2026-09-11): 精度同等・時間−25%・出典正確性、案A/案D/no_info無効
- [D-KB 順 1〜3 状態](dkb-order1-3-status.md) — 2026-09-15 完了・EC2 反映済(obscontract)。⑤ 既定化なし・提示切替なし(dec 0700)
- [federate 第1・2期完了](federate-phase1-2-20260915.md) — 2026-09-15: 精度72%は構造的上限・確定改善=再ランクキャッシュ
- [索引の行除去20260923](index-row-removal-20260923.md) — A索引3,913→3,906・refs 2,030→2,026を行除去で是正(埋め込み再計算なし)
- [T0 段階 1 試作(dkb)](t0-stage1-trial.md) — snapshot 2213・rev 第 3 次の dkb 分(U1/U2/T2/T3)是正済み
- [V3 第1段の受入条件](v3-stage1-acceptance-state.md) / [第1・2段の設計承認](v3-stage1-design-approved.md) — 承認2段分割・EC2反映+URL公開(2026-09-23)・辞書S0適用
- [V3 第 2 段 S3 計画の状態](v3-stage2-s3-plan-state.md) — 第 2 段の bkb 受入は全部完了(S7・凍V-c)・共通表 改訂 11 確定 30ba9359(決66)で引く
- [T0 探索木の画像デモ](t0-tree-image-demo-plan.md) / [デモ計画承認20260924](demo-plans-approved-20260924.md) — 19 問を EC2 配置・表示改善 第 1 段(決73〜79)
- [V3 第 2 段(S4-F〜S7)](v3-s4f-vocab-reader-state.md) — S7 完了(決63・2026-09-24)・EC2 は語彙 DB の経路(案 B の設定ファイル)
- [D-KB カバレッジ拡大(決80〜94)](dkb-coverage-facets-plan.md) — 2026-09-25: 段 1・表示側・旗 ON まで EC2 反映済み・alert 実例 3 問確認済み
- [D-KB カバレッジ×facets(決80)](dkb-coverage-facets-fed.md) / [検討](dkb-coverage-facets-20260925.md) — fed 案 fe980712・制約5・現象→症状の被覆 113/891
- [決80 bkb 分の実測](dkb-coverage-singularity-bkb.md) / [Document.text と masked の向き](b-document-vs-masked-files.md) — マスク 47 件は決86 で地名と判定・Document.text は地名の戻し
- [D-KB 段 1 の状態](dkb-stage1-singularities-state.md) — 特異点の alert と供給口を旗 OFF で実装・LOO 646 で C0〜C4・A2 合格
- [D-KB 特異点の表示側](singularity-display-plan.md) — 決90/92 承認・実装 1cfda730(Dify 0.3.9・demo_shirei・demo_v10
- [順の明示の受入(search・followup)](order-acceptance-search-followup-bkb.md) — 決107 実装・受入 完了(accident_backend 576820f1
- [D-KG の順の明示(完了)](dkg-order-dependence-item.md) — 決113〜122(2026-09-27〜28): dkg_backend a33d7443・受入 646/646
- [直接経路の content_filtered(完了)](direct-path-content-filtered-item.md) — 決124〜132(2026-09-28): 直送の口は Dify 状態整理/質問
- [facets 個体 entry の受入(bkb)](facets-instance-entry-acceptance-bkb.md) — 決134〜138・受入設計 改訂 7・独立投影は主番号ごと
- 受入(bkb): [甲・乙・丁・F1](c-conn-mark-acceptance-bkb.md) / [戊 照会の待ちの上限](query-timeout-bo-bkb.md) / [戊-d](bo-d-vocab-retry-acceptance-bkb.md) — いずれも合格(2026-10-06〜決230)
- [表示改善 第 3 段の受入(bkb)](display-stage3-acceptance-bkb.md) — S6 (c) を含め是正後に全部合格(2026-10)

## 2026-11-20 に移した項目(34 件・元の行のまま)

- [姉妹jreast-phase0](sibling-jreast-phase0.md) — JR東日本DWG連動図表→QAデモを別セッション(~/jreast-phase0)で立上げ・本成果を片方向コピー再利用
- [海鉄コメント対応20260826](kaitetsu-comments-20260826.md) — ②36問対応表+F-1読取強化をステージング検証済み・本体未投入(承認待ち)
- [federate vs D-KB比較](fed-vs-dkb-comparison.md) / [D-KB再発照合改善](dkb-recurrence-improvement.md) — 統一比較 fed72≈lite70>dkg52-56(eaf19f5)
- [数値基準整備HB/T2](quant-standards-hb-t2.md) — 定量化523件・定格72件(投入待ち)・Ⅴ編取替目安+T2規範値44件格納・次回LOOでjudge較正要
- [C系診断ビュー](c-diag-view.md) — Phase1実装済(U1鳴動逆引き/U3同時多発)・一元化維持・Phase2=影響範囲/鎖錠逆引きが残
- [ckb セッション状態](ckb-session-state.md) — P1/P2/P2b/P3 完了(kb_conn 化・ingest_diagrams G0〜G10・プロンプト正本・判定スコープ統一
- [C-KB 正答化 第1〜6弾](ckb-remediation-waves-20260916.md) — 2026-09-18 完了: 正準 179→186(保守 172→181・87.0%・誤答0)
- [B/D-KG 監査の状態](dkb-audit-state.md) — 2026-09-17 是正後 PASS 20/WARN 3(X6・X6b・X8=既知)/FAIL 0・入口コマンドと機械担保の所在
- [C-KB G7 の API 実費](c-kb-g7-api-cost.md) / [C-KB コスト削減20260918](ckb-costdown-20260918.md) — G7 1 回 73.6→約 24 ドル・cachePoint 化・速度は不変
- [障害探索ツリー fed](fault-tree-stage3-fed.md) — 第3〜5段: 読み方表=docs/設計_読み方表_障害探索ツリー_20260922.md(改訂1・§10停止条件
- [安全区分の印の表示](safety-mark-display.md) — Dify の文面はプラグイン dkg_diagnose.py が組み立てる・印は質問と別行・印なしは出力不変・v0.3.6
- [辞書の是正は閉じた](dict-remediation-closed.md) — 機械検査 770 件は是正しない・効かない理由=同定の枠が入れ替わる・閉じるときも測る
- [甲13 の決着(B-KB 併記)](bkb-koh13-outcome.md) — 「撤去しない」で決着・判定は保留(保1)で不合格0件
- [federate の B 照会も順に依る](federate-b-order-dependence.md) — 実装 959786cd・受入 段 0/2/3 合格・**評価完了 2026-09-27
- [本番の合成を Opus 5.5 へ](opus55-production-switch-item.md) — **2026-09-28 本番切替済み**(EC2 反映
- [Dify refusal_fallback(決131 閉じ)](dify-refusal-fallback-item.md) / [Dify 状態整理の後ろ盾(案 A)](dify-state-fallback-plan-a.md) — 決123〜132 で閉じた・EC2 反映済み
- [エージェント向け資料 3 本(20260928)](agent-docs-bkb-dkb-20260928.md) — B-KB query/facets ガイド・D-KB diagnose ガイド・改変の説明
- [技術説明会 資料(20260928〜1006)](tech-talk-materials-20260928.md) — 項目 1/3 md・Slides Artifact(版 9)・項目 1/3 PPTX
- [事故摘録の原本の API(bkb)](accident-rawdata-endpoint-bkb.md) / [EC2 反映(決154〜158)](accident-rawdata-api-item.md) — /v1/accident/rawdata
- [3 層辞書 確認シート新様式 S3(bkb)](dict-sheet-v2-s3-bkb.md) — 決161 S3 独立照合 33 検査一致
- 探索木: [内部状態の契約 P1](t0-tree-internal-state-contract.md) / [P2 計画 fed](dkg-tree-p2-plan-fed.md) / [第 1 段の受入 bkb](tree-stage1-acceptance-bkb.md) / [EC2 反映〜決231](t0-tree-stage1-ec2-deployed.md) — 受入合格・EC2 反映済み
- [explore 試作(決232〜241)](explore-proto-item.md) — /v1/explore の B/D ホップ探索・段 1 完了・改訂 14 は G1 不合格で段 2 停止
- [MVP1 最終報告・提出物](mvp1-final-report-slides.md) — 吉澤分スライド 5 枚版/ロング 11 枚・パイプライン仕様書 B/C/D(e94faaa51b6166a7
- [原因特定の手法の調査(agy/pla)](root-cause-methods-survey.md) — explore 閉鎖後の別手法調査 → 決247〜256
- [M02 段 0 の B 側](m02-stage0-b-side.md) — 決246・両方 21/50 だが 2 軸の類似は選別にならず 3 軸で 0・既存 b-3a と同じではない(2026-10-14)
- [停止点 3 の実装(完了)](stop-point-3-impl-pending.md) — 決263-A 受入合格・main 3599e9e1(2026-10-17)・次は決264
- [薄い窓口 A の状態](thin-plan-a-window-state.md) — 決264 の LLM 演習 完了(2026-10-18・不整合なし)・手順書・通しは正本の接続の環境変数を渡す
- [決275 探索木 V3 の受入(bkb)](tree-v3-acceptance-r275-bkb.md) — 決277〜286 合格・前段 [[tree-redesign-acceptance-r270-bkb]
- [Dify human-input の実測事実](dify-human-input-facts.md) / [決290 ボタン回答の受入(bkb)](dify-button-acceptance-r290-bkb.md) — カード内 mermaid 可・受入合格
- [決298 R1 判別の辺の受入(bkb)](r1-discriminates-acceptance-bkb.md) — 実装・是正・表示・決303 投入後まで合格(2026-10-24)
- [direct_causes と R1(決297〜304)](direct-causes-dead-questions.md) — 手元・EC2 の D-KG へ投入済み・続く F2
- [事故摘録ベクトル類似検索(決312〜325)](accident-vector-similar-item.md) — EC2 反映済み(t321)・決325 で表示は現状維持・正規化は今は変えない
- [新担当者向け 全体説明の資料(20261105)](agent-newcomer-briefing-deck-20261105.md) — SMテンプレ pptx 10 枚・build_deck.py
- [一括追加の社内現状説明の資料(20261110)](bulk-intake-status-deck-20261110.md) — SMテンプレ pptx 17 枚・build_deck.py・docs/資料_事故摘録の一括追加_現状説明_20261110
