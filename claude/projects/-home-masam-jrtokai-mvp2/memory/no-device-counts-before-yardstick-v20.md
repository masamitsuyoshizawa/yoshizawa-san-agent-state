---
name: no-device-counts-before-yardstick-v20
description: 決212 §3 の後、2a8c の機器の束ね・部分・名称表の問いの状態・番号の組・結びの数は連絡文に書かない(acc の物差し第 20 版の期待の前)・書くのは合否・変わりの有無・問いの数・秒・版・指紋・置き場(2026-11-24・coord)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-24T02:14:15.195Z
---

acc が、物差しの第 20 版で、2a8c の「機器の束ね・部分・接点の先の配線がつながる機器」を照らす計画を立てた(新しい欄の期待はまだ無い)。coord が、決195 と同じ形で、dgr に、これらの欄の 2a8c の読みの数を連絡文に書かないよう頼んだ(決212 §3・それまで dgr は rep に行を採った機器の数・部分ごとの数・組の変わりの数などを書いていた = coord の見落とし)。

**Why:** 受入の独立(CLAUDE.md §4)。期待を凍結する前に、読取の側の数を acc が見ると、期待が読取に寄る。
**How to apply:**
- 書いてよい物: 合否・変わった所の有無・問いの数・秒・道具の版・指紋・置き場。
- 書かない物: 行を採った機器の数・部分ごとの記号の数・名称表の問いの状態ごとの数・番号の組の数・結びの数・付く先の類ごとの数など、機器・部分・束ね・結びの欄の数。
- 値は git の外(data/derived/…)に置き、rep には置き場と sha16 を書く。
- 前の rep は書き直さない(coord が acc の答えを待って決める)。迷う数は、書く前に coord に一言。
- 解ける条件: acc の物差し第 20 版でこれらの欄の期待が凍結され、coord の知らせが来た時。
- 関連: [[frozen-means-expectation-version-not-document]] [[frozen-v1-not-frozen-for-new-fields]] [[acc-a-layer-values-before-freeze]]
