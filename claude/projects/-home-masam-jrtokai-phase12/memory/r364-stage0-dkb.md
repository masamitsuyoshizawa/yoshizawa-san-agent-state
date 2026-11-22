---
name: r364-stage0-dkb
description: 決364 推論 KB の段 0 の dkb の数え(2026-11-22)— 系統の表・Stage2 の対応の表・D1〜D6・進む条件に届く系統は 0
metadata:
  type: project
---

決364(推論 KB = 事故の「なぜ」の知識化・計画 docs/計画_決364_推論KB_coord_20261122.md・利用者が承認)の段 0 = 700 件で決定論の数えだけ(LLM 0)。

- 台本: 枝 dkb-r364 の knowledge_kb_v8/scripts/dkg/r364_stage0_dkb.py(family-table・stage2-map・counts・cross)。出力は本体ツリーの git 外 knowledge_kb_v8/data/eval/r364/(0700)。
- 系統 = LOO の事例の正解の系統(dkg_loo700_cases.jsonl・10-14 の段 0 と同じ)。系統なし 247・LOO の外 53・独立の群 602(accident_twins の連結成分)。
- Stage2 → acc は B の source_file で結ぶ(8-27 の抽出の道具と同じ)= 675/702・PDF の側は bkb の census の結び(350)を使う。
- 類例の有無は決355 段 A の手がかり(a1_signals.json・B 360406 の版)で、本文の類例の閾値は 0.6(仮の型 (b) と同じ・0.4 では 23 問食い違った)。
- 結果(11-22 16:29 の訂正後): D2 9 事故だけ・D2b 482・bkb の材料 388 との重なり 295・そのうち**測定済みの類例 0 は 93**(未測定 5)。全体の測定済みの 0 は 213・未測定 11(本文の類例 S2 が None の問いを 0 に数えていた = rev 1623 の高・15:20 の 98 と 224 は誤り)。進む条件に届く系統は 0。利用者の決め = 3 系統(軌道回路・電源・リレー)を合わせて段 1(材料 56・測定済みの 0 は 29)。

**Why:** 10-14 の壁(初めの知らせが薄い)が今回も同じ形で在るかを、数で見切る段。
**How to apply:** 段 1 の判じは coord と利用者。測れなかった物を 0 に数えない(未測定を別の札に)。数え直す時は、系統が 2 つの事故の数え方(それぞれに数えるか、1 つだけか)を必ず書く(bkb とずれた)。関連 [[why-reasoning-kb-r364]] [[reasoning-kb-r364-stage0-bkb]] [[rebuild-stageA-dkb-20261113]]
