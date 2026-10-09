---
name: xlsx-drawingml-shape-analysis
description: xlsxオートシェイプ(現示系統線等)の決定論解析法 — セル格子はアンカー↔xfrm対応から逆算する(T7で実証)
metadata: 
  node_type: memory
  type: project
  originSessionId: d7006476-b6c7-4c0e-96fe-5032710ef3e3
  modified: 2026-08-02T11:18:29.999Z
---

xlsx 内のオートシェイプ(線・楕円)を意味解析する場合(T7 信号現示図で実証):

- 列幅→px の OOXML 式(MDW)による格子推定は不正確で使わない。**各 twoCellAnchor の
  セルアンカー(col/row+colOff/rowOff)と絶対座標(a:xfrm の off/ext)の対応から
  列・行境界 EMU を逆算**する(boundary[col] = x - colOff の中央値)。行高がシートごとに
  違っても追従する。汚染観測があるため**単調増加フィルタ(LIS)必須**(T7 では drawing8 の
  行54 が汚染され許容値が負になった)。
- 線の端点は off/ext + flipH/flipV から復元。線種→意味は凡例行の図形位置で確定させる
  (T7: 黒実線=基本現示/青実線=中減速/黒破線=高減速/黒一点鎖線=連動作用)。
- 線はセル中心でなく**文字セル端⇔隣セル端**で描かれる → 中心距離でなく列区間包含でスナップ。
- 短い線(dx<40px)は矢羽・点などの装飾。図枠外へ抜ける線は継続フラグで保持し、
  頁間結合は推測しない。

**Why:** 式推定の格子では端点スナップが大量に失敗(マッチ29%)、アンカー逆算+区間包含で
99.7% に到達した。

**How to apply:** scripts/t7_genji_extract.py の calibrate_grid / snap_x / snap_y を参照。
関連: [[pdf-cid-font-recovery]]
