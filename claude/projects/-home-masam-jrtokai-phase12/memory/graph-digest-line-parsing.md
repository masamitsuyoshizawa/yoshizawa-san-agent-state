---
name: graph-digest-line-parsing
description: graph_digest の書き出しの行は辺の両端の id をエスケープせず「|」で区切るので、そのまま split すると取り違える(改行 id・原因概念の id)
metadata:
  node_type: memory
  type: reference
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-29T15:59:46.872Z
---

`knowledge_kb_v8/scripts/dkg/export_graph_apoc.graph_digest(dump=…)` の行の形(2026-10-30 に dkb が rehearse_guard の K-2c で確かめた・確定)。

- 節点の行 = `N|<ラベルを昇順に : でつなぐ>|<_props の文字列>`。文字列の値は `\\`・`"`・改行・復帰をエスケープする。
- 辺の行 = `R|<型>|<a.id>|<b.id>|<_props の文字列>`。**両端の id は生のまま**(エスケープしない)。id の無い節点は `None`。
- そのため 2 つの落とし穴がある。
  - 原因概念の id が `cc:<設備>|<述語>` と「|」を含む(B の CauseConcept 221 個)。`split("|", 4)` では終点を取り違える。
  - 改行を含む id(手元の B の Equipment 11 個・辺 38 本・D-KG の CanonicalSymptom 3 個)では、1 本の辺の行が書き出しの上で物理行に割れる。
- 読み方: 「N|」「R|」で始まらない物理行を前の行の続きとしてつなぎ直し、節点と辺の数を `graph_digest` の戻り(n, r)と照らす。辺の両端は、節点の行と同じエスケープをかけてから節点の id の一覧に照らし、一意に決まるときだけ切る(`rehearse_guard._read_lines`・`_esc`・`_split_r`)。
- 性質の文字列は `rehearse_guard._parse_props` で辞書に戻せる(手元の 41,734 行で往復一致)。

関連: [[graph-query-discipline]] [[hash-match-not-completeness]]
