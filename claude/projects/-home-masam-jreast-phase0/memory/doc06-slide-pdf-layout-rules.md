---
name: doc06-slide-pdf-layout-rules
description: 横スライド PDF(経営ビジョン等)を PyMuPDF で決定論構造化する際の落とし穴と規則(Doc06 で実証)
metadata:
  type: project
---

スライド PDF のテキスト層は「読み順が信用できない」だけでなく、以下の癖がある(Doc06 の 3 文書、2026-08-31 実証):
- 縁取り/影付き文字は同文言が同座標(または y が数 pt ずれ)で二重に入る → (bbox 丸め, text) 完全一致と「同文言・x±1.5・dy<size*0.6」の近接重複を除去。
- 縦書きタブや強調語は 1 文字 1 スパン → 同 x±2・同 size・y 連続(重なり許容 -size*0.5)で縦連結。行頭記号(■●・①)は縦連結から除外しないと「●●●●」ブロックになる。
- 行頭記号が本文より後のスパン順で来ることがある → 行結合は右隣接だけでなく左隣接(gap を両向きで評価)も許容。
- 頁題は最大級フォント(24pt/22pt)で頁上部固定 → role=title は size と y<60 で決定論判定できる。一部文書は頁番号も同寸で x<60 に来るので NUM_ONLY で除外。
- 棒グラフの数値は「2」「倍」「N,NNN」「FY2023」に分割される → 半面(x で分割)ごとの座標対応の頁固有規則(prov=layout-rule)で復元。
- ロードマップは年軸ラベルの x から線形補間で年次推定(conf=medium)し、同一矩形内の行は 1 項目に結合。

**Why:** LLM に頼らず 99.5% 以上の文字保全で Block 化でき、意味層(柱→施策→数値→年次)を規則だけで導出できた(vision は表紙・区切り頁のみ)。
**How to apply:** `scripts/doc06_layer1_extract.py` の merge_vertical / build_lines / assign_roles と `doc06_build_semantic.py` の beyond_numeric_targets / roadmap_items を雛形にする。漏洩監査で「正解文言の直書き」が検出されたら、他文書の文言完全一致など規則に置き換える。関連: [[pdf-cid-font-recovery]] [[nfkc-tilde-pitfall]]
