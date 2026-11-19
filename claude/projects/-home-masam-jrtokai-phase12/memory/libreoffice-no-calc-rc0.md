---
name: libreoffice-no-calc-rc0
description: 元 PC の LibreOffice は表計算(Calc)が無く、soffice --convert-to は xlsx・csv も読めず「source file could not be loaded」で rc 0・出力なしになる(2026-11-09 確定)
metadata:
  node_type: memory
  type: reference
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-08T18:02:58.548Z
---

2026-11-09(決352 段 2 の 6)に確かめた。元 PC の WSL に入っている LibreOffice 24.2.7.2 の部品は、base-core・draw・impress・math・writer だけで、calc は入っていない(`/usr/share/doc` の一覧で確定)。

- `soffice --headless --convert-to xlsx|xls` は、xlsx も csv も「Error: source file could not be loaded」を出す。それでも **rc は 0** で、出力のファイルを作らない。囲い(sandbox)を外しても同じなので、囲いのせいではない。
- 変換の成否は、rc ではなく出力のファイルの有無で判じる。`excel_text.convert_xls` はそうしている。
- **xls は群 C の 887 件すべて**(段 3 の 50 件のうち 15 件・coord rep 20261109-1736 で確定)。bkb の「0 件の見込み」は誤りだった(設計 §3 の表が xlsx と xlsm だけを数えていた)。決352 追補 12 で利用者が `libreoffice-calc` 24.2.7 を入れると判断した。入ったら coord が知らせる → E10r と段 3 の 15 件の範囲の確かめ(3 つの sha・版・本文の sha の照らし)と、群 C の分類の数え直し。
- `test_excel_text_bkb.py` の E10r(本物での変換)は、Calc が無いと未実施になる(合格に数えない)。

関連: [[bulk-intake-r352-bkb]]
