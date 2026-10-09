---
name: shirei-instruction-kb
description: 指令指示支援グラフ化P1-P4完了(通知837/機材151/措置371/規制392)・actionsフルセット化・P5不実施
metadata:
  type: project
---

指令指示支援の依拠文書グラフ化(計画20260908・全クローズ):
P1 NotifyRule837+NotifyParty14(M1/M2の宛先・行為・内容・条件)/P2 EquipmentKit151(持参機材)/
P3 RemedyAction371(条件→応急・復旧措置)/P4 OperationRule392(条件→運転扱い+解除条件)。
P5(併発示唆)は18件のみ・既存カバーで不実施。抽出は全てEC2大阪経路([[ec2-bedrock-fallback]])。
actionsは 安全措置(運転扱い実ルール込み)→通知→持参機材→応急措置→確認→測定→手順 のフルセット。
照合は現象コード対象+同型事象語一致優先(弱関連1件まで)。「最終判断は指令の規程による」を明記。

**Why:** ミッション拡張=原因究明+指令の的確な指示(現場・関係者通知)支援。
**How to apply:** 弱関連抑制の重み調整は_notify/_kit/_remedy/_oprule共通パターン。新文書受領時は
同方式(FILTER見積→EC2抽出→span検証→target_codes照合)で拡充。関連=[[dkg-cause-kg]]
