---
name: c-diag-view
description: C系診断ビュー照会層 全フェーズ完了(U1-U4実装・Phase3給電は原資料に情報なしで不実施=給電系統図の提供待ち)
metadata:
  type: project
---

C系連携強化(2026-09-05承認・検討_C系連携強化_診断活用_20260905.md): C系(9990)は一元化維持・複製しない。
kg_api/kb/scripts/c_diag_view.py = 読み取り専用の診断特化照会層(lru_cacheで起動時ロード)。
Phase1実装済: U1=踏切名→鳴動依存TC列挙(SoundingRule/TRIGGERED_BY・実TC名で確認質問を追補)、
U3=複数箇所→共通TC・キロ程近接(179k557m形式パース)で同時多発の決定論判別。
diagnose応答キー=c_insights。shireiデモ「図表KGからの手がかり」欄。

**Why:** 実査でC系は鳴動/鎖錠/トポロジーが既エッジ化済み=不足は診断側活用層のみと判明。
Phase2実装済(2026-09-05): U2=接地TC→影響先(CONTROLS_STOP_OF信号/ROUTE_LOCKS/鳴動踏切)、
U4=駅名+信号記号→進路の通過TC(PASSES_TC=てっ査鎖錠)+LOCKS_SWITCH転てつ器。
進路記号はRoute名先頭から決定論抽出(route_symbolは未設定)・駅名は「駅」末尾除去で正規化。
Phase3調査済(2026-09-05): RawDiagram320件に給電・器具箱情報は実質ゼロ→不実施(創作しない)。
**How to apply:** 給電系統図・配線略図が提供されたらPhase3再開。C系書き込みはその承認後のみ。関連=[[dkg-cause-kg]]
