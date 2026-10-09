---
name: hash-match-not-completeness
description: hash 一致は完全性ではない — 欠けた中身同士でも一致する・「allowlist 外」と「あるべきキーの欠け」の両方を見る・検証の一部しか回していなければそう書く
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-15T12:59:47.641Z
---

二者の hash が一致しても、エラー 0 でも、**欠けた中身同士なら一致する**。検査は「allowlist 外のキーが 0」と「**あるべき(必須)キーが出力に揃う**」の両方を置く。**検証の一部(例: V1・V3 だけ)しか回していなければ、報告にそう書く**。

**Why:** 2026-09-23 の T0 snapshot t0-20260915-2148 で、dkb の抽出器が「キー名が `_ref` で終わるか」で ref 属性を判定し `prov_ref` を捨てた。dkb の C3 は D-KG 側の非空だけを見ており、fed の V1 も「allowlist 外 0」だけを見ていたため、両者の hash 一致・エラー 0 のまま coord へ「合格」と報告した。overlay の参照解決(closure の required_props)で初めて見つかり、2156 に取り直した。

**How to apply:** 写す/置換する属性の判定は名前の接尾辞でなく明示リストで行う。抽出側は「必須キーが出力に届いたか」を検査し、検証側は allowlist のキーの欠けを warnings・参照閉包の必須キーの欠けをエラーにする。テストに出力側のキー集合の一致を入れる。hash の突き合わせは同一性の確認であって完全性の確認ではないと書く。

(このファイルは fed が先に作成し、dkb が読まずに上書きしたため、fed の索引の要点と dkb の教訓を合わせて書き直した・2026-09-23)

関連: [[verify-zero-counts-before-reporting]]・[[t0-stage1-trial]]

**報告の書き方(fed の元の本文から)**: 検証の一部しか回せなかったときは、回していない段を明記して報告する(2026-09-23 は overlay が無く、required_props を見る V2 を回さないまま「V1・V3 エラー 0」と coord へ伝え、後で訂正した)。「エラー 0」は何を見ていないかとセットでしか意味を持たない。
