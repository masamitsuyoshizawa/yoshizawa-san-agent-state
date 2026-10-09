---
name: doc05-red-text-diff-and-header-matrix
description: 現改比較表の差分は PyMuPDF の span color(0xFF0000)で決定論に取れる。○表の列見出しは「款行+項行」の2段や列見出しにしか現れない項があるため列見出し自体をノード源にする(Doc05で実証)
metadata:
  type: project
---

- 現改比較表(現行/改正の対比)は改正側の変更語が赤字。`page.get_text("dict")` の span["color"]==0xFF0000
  で赤字スパンを取れば差分位置が決定論で決まる(254 スパン/34 頁)。段落対は左右の y 範囲の重なり(≥0.3)で
  対応付け、赤字無し・同文・（略）行は「改正なし」。縦書き列見出しの改正は文字を縦書き順に連結して保持(低信頼)。
- ○●◎ マトリクス頁は (1) 款コード行が項コード行の直上にある 2 段見出し(列ごとに款が違う)、
  (2) 列見出しにしか現れない項(本体に行が無い)がある。列見出しの縦書き名を読んで項ノードを補い、
  2 段見出しは「款-項」をキーにしないと対象なしが数百件出る(741→0)。
- 別表(一覧)にしか無い目節(別表4 のみ 83 件)はスタブ科目(prov=beppyo4-only・低信頼)で保持すると網羅性が保てる。

**Why:** 差分をテキスト比較(difflib)で出すと折返し位置の差を誤検出する。色情報は原本が持つ確定情報。
**How to apply:** `scripts/doc05_extract_genkai.py`(赤字)、`scripts/doc05_extract_beppyo12.py` detect_header(2段見出し)、
`scripts/doc05_load_graph.py` load_beppyo4(スタブ)。関連: [[doc05-centered-cell-band-recovery]]
