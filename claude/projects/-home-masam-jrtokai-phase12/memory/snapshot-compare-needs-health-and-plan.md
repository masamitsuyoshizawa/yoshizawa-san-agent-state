---
name: snapshot-compare-needs-health-and-plan
description: 変更前後の写しの「N 件同じ」は予定の集合と採取の健康を検査してから言う・コードの写しは kg_api の外(knowledge_kb_v8/config・data)まで揃える(決230 R230-2・2026-10-10)
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-10T05:20:27.661Z
---

2026-10-10 の決230 で、比較用のコードの写しに `knowledge_kb_v8/config`(安全区分の台帳)を置き忘れ、木の写しは前後とも全 56 件が `ledger_unreadable` で根の停止だった。旧来の比較道具(`tree_snapshot.py compare`)は「56/56 同じ」と出し、私も一度それを同一の根拠に数えかけた。rev(rep 20261010-1349 R230-2)は、比較が予定の集合・重複・両側の同じ欠落・採取時の健康を見ていないことを指摘した。

**Why:** 差と欠落を数えるだけの比較は、両側が同じように壊れていても(同じ欠落・同じ障害応答・同じ根の停止)「同一」と出す。同一は不変の証拠にならない。

**How to apply:**
- 前後比較は `knowledge_kb_v8/scripts/dkg/snapshot_verify.py`(予定の集合を写しと独立に作る・重複・欠落・予定外・読めない本文・行ごとと全体の健康・差 → 外れたら終了値 1)を通してから「同一」と書く。否定例は `test_snapshot_verify.py`。
- 写しを `git archive` や `git ls-files` で作るときは、コードが読む git 外・kg_api の外の参照(`kg_api/kb/data`・`kb/index`・`kg_api/data`・`knowledge_kb_v8/config`・`knowledge_kb_v8/data`)を揃え、回す前に健康の件数(根の停止 0・C の接地あり・項目の選択あり)を見る。
- 変異の検出は「期待した試験が FAIL で落ちた」で数え、表(置換・試験・理由)をリポジトリの実行器に置く(`mutate_bo230.py`)。
関連: [[hash-match-not-completeness]]・[[criterion-not-tool-output]]・[[c-grounding-silent-failure]]・[[negative-test-check-reason]]

**追記(2026-10-15・dkb 表示契約 D 側 T1)**: 固定の作業ツリーに `kg_api/kb/data` が無く、前後とも意味照合が黙って失敗して「同一」と出た(同じ壊れ方は差分に出ない)。**意味照合の失敗 0 行を合格の条件に入れる**。snapshot_verify の健康の検査は意味照合の失敗の印を見ていない(道具の改善候補)。関連 [[gpu-contention-silent-semantic-failure]] [[silent-path-drift-masks-cause]]