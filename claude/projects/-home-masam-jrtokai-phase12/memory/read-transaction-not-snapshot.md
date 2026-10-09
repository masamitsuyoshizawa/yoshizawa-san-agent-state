---
name: read-transaction-not-snapshot
description: 「1 つの read transaction で読めば一貫した時点が見える」は DB の分離水準を実測するまで契約に書かない(語彙 DB の Neo4j 2026.03.1 は read committed で途中の公開 commit が見えた・2026-09-24 決55)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-24T04:24:10.065Z
---

V3 第 2 段の設計(決30-1・決31-3)は「束の読込みは 1 つの read transaction で行えば交錯が消える」と書き、rev の 4 巡でも通ったが、dkb が隔離 DB で筋書き 6f(P/R を読む → 公開 commit → 値を読む)を実走したところ、Neo4j 2026.03.1 は read committed で、同じ read transaction の中の後の照会が途中の commit を見た。契約は「読み終えたら P/R を読み直し、違えば束を捨てて読み直す(BUNDLE_RACE)」に置き換えた(決55・設計書 追補版 11 638f122e3635f4ed・共通表 次の改訂 69)。設計書自身が「まだ確かめていない」と限定を書いていたのが救い。

**Why:** transaction の分離水準は DB と版に依り、紙上のレビューでは決まらない。一貫性を transaction に頼る設計は、実 DB の実走(交錯の否定例)を終了条件に入れないと、実装後に静かに壊れる。

**How to apply:** 一貫した読取りを要る設計では「分離水準を実測した」か「値の再読で担保する」かのどちらかを書く。実測前は「未確認」と限定を残し、S4 相当の終了条件に交錯試験を入れる。[[spec-unverified-until-implemented]] [[graph-query-discipline]]
