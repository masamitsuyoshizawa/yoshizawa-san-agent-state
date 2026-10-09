---
name: annotations-must-read-original-input
description: 質問への併記・前処理を数珠つなぎにすると後段が前段の出力を食う。いずれも元の入力を見る形にする
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-16T12:04:38.740Z
---

`kg_api` の `ep_ask` は accident の質問に 3 つの併記(表記ゆれ・現象語・設備名)を足す。これを
`req.question = f3(f2(f1(q)))` と数珠つなぎにすると、**後段が前段の足した文字列を入力にする**。

2026-09-27 に実害が出た。`FM_HOWTO`(現象語の併記に畳んだ使い方の文)に `MATCH` という語があり、
そこに **`AT` が含まれる**。`AT` は実在する `Equipment.name`(1 件)で、設備名の検出は
`$q CONTAINS e.name` なので当たる。結果、**設備と無関係な現象寄りの問いに `AT` の絞り込みが入り、
述語 Recall が 1.0000 → 0.0000**(取得 0 行)になった。

**是正**: `_annotation_suffix(fn, q)`(併記関数が足した部分だけを返す)と `_annotate_accident(q)`
(3 つとも元の質問 `q0` だけを入力にし、足りた分を連結)。`ep_ask` は後者を 1 回呼ぶだけ。

**Why:** 前処理が互いの出力を食うと、片方の文面を変えただけでもう片方の判定が変わる。文面は
指示のために長くなりがちで、そこに短い識別子(`AT`・`ES` のような 2 文字の設備名)が偶然含まれる。

**How to apply:** 入力へ注釈を足す処理が複数あるときは、**いずれも元の入力だけを見る**設計にする。
検査も**本番と同じ順序で通して**見る — 単体で `f3(q)` だけを試すと、この欠陥は再現しない
(実際、実装側の確認も試験の検査も単体で見ていたため、本番でしか出なかった)。
関連: [[prompt-hints-apply-to-every-request]] [[negative-test-check-reason]]
[[claim-wider-than-implementation]]
