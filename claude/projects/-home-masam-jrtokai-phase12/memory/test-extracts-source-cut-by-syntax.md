---
name: test-extracts-source-cut-by-syntax
description: 実装ソースを切り出して動かす試験は、境界を構文(ast)で取り、定数を試験側で作り直さない
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-16T09:31:31.823Z
---

`kg_api/kb/scripts/test_*.py` は FastAPI やドライバを import せずに `app.py` の関数だけを動かすため、
ソースを切り出して `exec` する。このとき **正規表現で「次の `def` まで」と切ってはいけない** — 間に
モジュール直下の定数が挟まると巻き込む(`_annotate_halfwidth` の直後に `_FM_LOCK = threading.Lock()`
を足した瞬間に `test_halfwidth_kana` が NameError で落ちた)。**`ast` でモジュール直下の定義の範囲を取る。**

**定数も `app.py` から取る。試験側で作り直さない。** `_KATAKANA_RUN` を試験が自前で定義していたため、
実装側の正規表現を変えても試験は古い定義のまま通る状態だった。

**Why:** 切り出しの境界は実装の並び順に依存するので、無関係な追加で壊れる。再定義は「試験環境が本番より
寛容だと欠陥を隠す」型そのもので、守りたい検査が効かなくなる。

**How to apply:** 切り出しは `ast.parse` → `tree.body` の `FunctionDef`/単純代入を名前で選び
`ast.get_source_segment` で取る。依存の順に `exec` する。生成コードを文字列で照合するときは
`ast.unparse` が文字列を**シングルクォート**で出すことに注意(`graph == 'accident'`)。
関連: [[negative-test-check-reason]] [[artifact-self-report-not-trusted]] [[verify-in-separate-process]]
