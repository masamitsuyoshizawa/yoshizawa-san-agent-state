---
name: dict-sheet-v2-s3-bkb
description: 決161 S3 3 層辞書 顧客確認シート新様式(粒度列)の bkb 独立照合の結果と道具
metadata:
  type: project
---

2026-10-01 決161 S3: 回付版 f46bbd57ed406a69 を入力(辞書 338ea0b1・台帳 80d9c595・9/14 回付版 b595d531・計画 改訂 1 eac02d17 の P1〜P3)から独立に照合し 33 検査すべて一致(rep 20261001-1359)。道具 knowledge_kb_v8/scripts/bkb/check_dict_sheet_v2.py(--new で壊した写しを渡せる・否定例 5)。

- 辞書の別名は `{surface, kind, status, …}` の dict(文字列と思って最初に 53/47 の不一致を出した)。
- 説明シートには氏名検査の説明の文として `[氏名]` が 1 つある(データの痕跡ではない)。
- req の 9/14 回付版 sha16 0754adea は 9/14 の info の値で、実物は b595d531(並びがハッシュの種で変わる道具)。
- bkb は氏名の補完辞書を読まないので、名簿による X9 は dkb の K8 にしか担保が無い。
- 次: S5(返却の取り込み)の前に、bkb の採用札生成に `customer` を足す・受K2/受K2b に採用札 v2 の独立照合を足す(R159-1・別承認)。

**Why:** 生成器の出力を数え直すだけでは出力自体の誤りが見えない(rev R159-4)。
**How to apply:** S5 で返却版を受けたら同じ道具の考え方(入力から期待)で採用札 v2 を照合する。関連 [[facets-instance-entry-acceptance-bkb]] [[collation-derive-expected-from-source]]
