---
name: check-canonical-state-before-writing
description: 「正本に入っている」は、台帳 graph_registry.json が指す書き出しの Document の節点で確かめる・pipeline_state には図面など文書でない物も入る(2026-11-16 info 1357・2026-11-17 rep 2220 で 135 と書き coord が 126 と正した)
metadata:
  type: feedback
---

info 1357 で、事例報告の前の 2 本を「正本に入った前の 2 本」と書いた。kgb が工程の状態のファイルを見て、正本に入っていないと正した(kgb rep 1403・決126 §2)。今の正本 122 本に事例報告は無かった。決115 の 5 枚の時は pipeline_state を確かめて書いていたのに、今回は書き出しの記録(kgb_input がある)から推して書いた。

**Why:** 正本に入っているかは、kgb の入れ替えか加える投入かを分ける。誤ると kgb の手番と dec の形を取り違える。
**How to apply:** 連絡文に「正本に入っている/いない」を書く前に、`data/graph/pipeline_state/<sha16>.json` の有無を道具で真偽として出す。kgb_input があることは、正本に入っていることを意味しない。[[verify-unfrozen-drawing-by-boolean]]

追補(2026-11-17・rep 2220 → 2223): pipeline_state の mode=commit かつ K7 ok を数えて「正本の本 135」と書いた。そのうち 9 は図面など文書でない物で、正本の文書は 126 だった(coord の控えと照らして指摘された)。
**How to apply(改め):** 正本に在る文書は、`data/graph/graph_registry.json` の `kg.export` が指す書き出し(`data/graph/export/kg_*.jsonl`)の Document の節点の id で数える。状態のファイルは「処理が通った」の印で、文書の正本の数ではない。両方を照らし、差の置き場も出す。
