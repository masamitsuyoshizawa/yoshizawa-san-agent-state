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
