---
name: accident-originals-pdf-and-excel
description: 事故摘録の原本は acc:0001〜0348 = PDF・acc:0349〜0700 = Excel(.xlsx)・B-KB の source_file は工程が読んだ PDF の名で原本の名ではない・原本の名と形式は rawdata/manifest.json から引く
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-11-05T06:45:04.055Z
---

事実(2026-11-05 に確かめた・確定):
- ご提供の原本は 2 回: 1 回目 = PDF(acc:0001〜0348・うち 2 件は同じ PDF が 2 フォルダに在る写し)/ 2 回目 = Excel .xlsx(acc:0349〜0700・352 件・フォルダの区分なし)。決154 の「PDF 348 件分・Excel 352 件」と同じ。
- 工程は Excel の原本も PDF にしてから読んだ。**B-KB の Accident.source_file は工程の PDF の名**(700 件すべて .pdf・原本の名とは拡張子を除いても 700 件すべて違う)。
- 原本の名と形式の正本 = 本体ツリーの `accident_kb_v7/data/rawdata/manifest.json`(items[].files[] の role=canonical の file_name・ext・sha16 2478b4119a46da39・2026-09-30 作成・未解決 0)。census_full.json の idmap は格納のフォルダの区分。

**Why:** 2026-11-05 の事故摘録の一覧の Excel で、source_file を原本の名と思い込んで出し、利用者に「Excel が混ざるはずが全部 pdf」と指摘された。
**How to apply:** 原本の名・形式を問われたら manifest から引く。一覧の道具 = `scripts/export/export_accident_list_excel.py`(出力は git 外 knowledge_kb_v8/data/export/accident_list_<日付>/)。関連 [[accident-rawdata-endpoint-bkb]]・[[kb-excel-export]]。


(索引から移した説明・2026-11-07)
- [事故摘録の原本は PDF と Excel](accident-originals-pdf-and-excel.md) — acc:0001〜0348 = PDF・0349〜0700 = Excel・B の source_file は工程の PDF の名・原本の名は rawdata/manifest.json
