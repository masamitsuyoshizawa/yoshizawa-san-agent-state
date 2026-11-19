---
name: pdf-tounicode-shared-glyph-fact
description: PyMuPDF で IPA 字形を描くと、字形を共有する字(半角空白・ハイフン・〜 など 13 字)が ToUnicode で別の字に当てられ、読み手は別の字を返す
metadata:
  type: reference
---

PyMuPDF 1.27.2.3 の insert_text で IPA ゴシック / IPA 明朝(TrueType・Identity-H)を使うと、ToUnicode は字形ごとに 1 つの字しか持てず、字形を共有する字の組(12 組 24 字)では片方に当たる。結果、半角の空白 → U+00A0・ハイフン U+002D → U+00AD(ソフトハイフン)・〜 U+301C → U+FF5E・— U+2014 → U+2015 など 13 字が、pdfplumber でも PyMuPDF でも別の字として読まれる(2026-11-17・合成で確定・道具 knowledge_kb_v8/scripts/bkb/dist_pdf_reader_probe_bkb.py)。位置の揺れは 0。

**Why:** 「空白を除いて比べる」照らしでは U+00A0 も空白として除かれるので見えない。完全一致の照らしを作って初めて分かった。
**How to apply:** PDF の字の層を照らす時は、空白を含めて字のコードまで比べる。直すなら描いた後に ToUnicode の bfchar(と bfrange)を文書で使った字に書き直す(試しで一致)。正規表現で書き直す時は bfchar の区画だけ(bfrange の行を壊した)。[[bulk-intake-r352-bkb]]
