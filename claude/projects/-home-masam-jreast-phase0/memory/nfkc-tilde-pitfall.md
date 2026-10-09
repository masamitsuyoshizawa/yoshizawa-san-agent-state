---
name: nfkc-tilde-pitfall
description: NFKC正規化は全角チルダU+FF5EをASCII~に写像し、区間名「A～B」照合が0件化する(T5で実証)
metadata: 
  node_type: memory
  type: project
  originSessionId: 8b3a18b7-11be-497b-bb73-d123fb40b339
  modified: 2026-08-02T11:12:54.514Z
---

Python の `unicodedata.normalize("NFKC", …)` は全角チルダ「～」(U+FF5E)を
ASCII `~` (0x7E) に写像する(波ダッシュ U+301C ではない)。JR図面の区間名
「駅U～駅V」等を NFKC 後に「～」リテラルで照合すると 0 件になる。

**Why:** T5 装柱抽出でパネル検出が 145→37 に激減する不具合の根因だった
(scripts/t5_souchu_extract.py の norm() で `~`と`〜`を U+FF5E に統一して解決)。

**How to apply:** 日本語図面テキストを NFKC 正規化する場合は、直後に
`.replace("~","～").replace("〜","～")` で波ダッシュ類を統一してから照合する。
形鋼寸法の罠([[t5-qty-parsing]]相当: L75×9/H200×150 は数量でない)と併せて
装柱系抽出の定番チェック項目。
