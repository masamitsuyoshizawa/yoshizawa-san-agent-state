---
name: mention-facts-belong-on-edges
description: 共有の実体ノードに「言及の事実」(原文表記・欄の注記・取り違えの印)を置くと 2 件目で上書きされる・evidence と同じく辺に置く(決336・2026-10-29)
metadata:
  type: project
---

決336(2026-10-29・利用者決定 B): build_graph.py が場所の節点(駅・区間・踏切・AlarmSite)に ON CREATE の外で `SET p += $extra`(name_raw・track_note・misfield)を書いており、2 件目の事故で上書き(絶対規則 3 違反・dkb が受入の照合中に発見)。3 つは「場所の性質」ではなく「その事故の文書がその場所をどう記したか」の事実 = 既存の辺が持つ `evidence`(根拠 span)と同じ型 → **辺の性質へ移す(スキーマ加法)**。A(ON CREATE に移す)は違反を止めるが、最初の事故の値が節点に固定され投入順で値が変わる・2 件目以降を捨てる。

**Why:** 共有の実体ノードは 1 つしか値を持てない。言及ごとの事実をそこに置くと、上書き(破壊)か先勝ち(順序依存)のどちらかになる。
**How to apply:** MERGE 文で ON CREATE の外に SET があれば「その値は誰の事実か」を問う。言及・出典・抽出の文脈は辺(evidence の隣)へ。既存データは 0 件なら移行せず新規投入から。refactor/修正の正しさは偽の tx で記録した tx.run の並びの前後比較(byte 同一・差分が期待どおりだけ)で示す(bkb の test_build_graph_refactor.py の型)。

**実装完了(2026-10-29 12:0x・dkb)**: main 218155327・build_graph.py 189bc0b5df8fe6e0(場所の節点は ON CREATE だけ・辺に name_raw/track_note/misfield)・機械の証 = 切り出し後の版の記録に書き換えを当てて byte 同一(合成 19/19・全件 702/702)・一時 DB 10/10・受入設計 改訂 33・設計確定メモ §14 に加法。EC2 は W2。


(索引から移した説明・2026-11-07)
- [言及の事実は辺に置く(決336)](mention-facts-belong-on-edges.md) — build_graph の name_raw 等を節点から辺へ・ON CREATE の外の SET は「誰の事実か」を問う
