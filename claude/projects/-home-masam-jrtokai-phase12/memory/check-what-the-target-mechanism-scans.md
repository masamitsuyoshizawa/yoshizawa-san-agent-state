---
name: check-what-the-target-mechanism-scans
description: 統制や門に項目を足すとき、足す先の仕組みが何を全件見るかを先に確かめる(G0 は manifest の files を全件照合)
metadata:
  type: feedback
---

統制の manifest や門に項目を「足す」前に、足す先の仕組みが**その鍵を誰がいつ全件で読むか**を現物で確かめる。

**Why:** 2026-09-24、辞書の生成入力 4 つ(term_index.json 等)を `pipeline_manifest.json` の `files` に足す案で fed と合意しかけたが、`ingest_pipeline.py` の G0 は `files` を全件照合するため、索引の更新のたびに B の投入が止まる形だった(意図は「辞書を生成し直すときだけ効かせる」)。別の鍵 `dict_inputs` を設け G0 は読まない形に変えた。fed も「同じ型で 3 度目」と記録。

**How to apply:** 「意図」を書いたら「その鍵で意図が実現できるか」を、読み手のコード(`for f, h in man["files"].items()` のような全件走査)で確かめてから合意する。投入の門と生成の門のように、効かせたい場面が違うなら鍵を分ける。関連: [[approval-line-for-own-artifacts]]・[[verify-what-the-target-actually-reads]]

**2026-09-24 にも同型(fed)**: 決41 の T0 画像デモの 3 本を `knowledge_kb_v8/scripts/fed/ft0/` に置き、段階 1 の照合器(置き場の *.py を全部依存に拾う)が FAIL 1 のまま数時間気づかなかった。**既存の置き場にファイルを足す前に、その置き場を全件走査する検査が在るかを grep する**(`glob` / `listdir` / `*.py`)。

**同日もう 1 件(fed)**: T0 デモの計画で「質問の追加は再起動なし」と書いたが、受け手の口は木の hash を manifest の**最上位の 1 つの snapshot** と照らしていた。snapshot の違う型板の質問では 503 になる。**「足せる」と書く前に、受け手の照合が何を 1 つだと仮定しているか(snapshot・版・置き場)を確かめる。**

**同じ日の 3 件目(fed・20:2x に発見)**: 同じ置き場の `ft0_tree_demo.py` は、もう 1 つの全件走査 `test_ft0_isolation.py`(`glob("ft0_*.py")` に対して通信系の import を禁じる)にも掛かり、W6(11:21)から FAIL のままだった。1 件目の照合器を直した時点で、**同じ置き場を拾う他の検査を洗い出していなかった**。**全件走査に 1 つ当たったら、同じ置き場を拾う検査を全部 grep して回す**(`*.py` 以外の名前の型、たとえば `ft0_*.py` も)。範囲の変更は主張を減らすので coord へ req(20260924-2028)。
