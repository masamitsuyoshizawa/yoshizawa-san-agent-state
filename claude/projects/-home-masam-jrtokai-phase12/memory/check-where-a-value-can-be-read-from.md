---
name: check-where-a-value-can-be-read-from
description: 読み手の条件を書くとき「その値がどこから読めるか」(ファイルか DB か・集合か最大 1 つか・同じ transaction で読めるか)を現物で確かめる
metadata:
  type: feedback
---

読み手の条件に「X の集合を Y から読む」と書く前に、Y が本当にその形の値を持ち、その読取りの単位(transaction など)の中で読めるかを現物(registry の形・kb_conn の作り)で確かめる。

**Why:** 2026-09-24、決30-3 の追補で coord が「P = 台帳の published な release_ord の集合(束の読込みと同じ read transaction で 1 回)」と書き、fed もそれを写した。台帳(graph_registry.json の vocab)は公開済みの最大 1 つ・4 鍵しか持たず集合でなく、ファイルなので DB の read transaction の中では読めない(dkb の指摘)。正 = P は DB の VocabRelease(published: true)から・台帳は R と digest(dec 20260924-0203)。

**How to apply:** 条件文の中の「〜から読む」ごとに、出どころの現物(ファイル/DB・値の多重度・読取り単位)を 1 行で併記する。dec を写す側(fed)も同じ検査をする。関連: [[verify-what-the-target-actually-reads]]・[[check-what-the-target-mechanism-scans]]
