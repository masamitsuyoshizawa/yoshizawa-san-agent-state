---
name: commit-tool-before-writing-handoff
description: 下流に渡す物(kgb_input など)は、道具をコミットしてから書き出す。試しはメモリの上で行い、本物の置き場に書かない(2026-10-30・dcr が _meta.tool.commit を食い違わせた)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 28bdbe4c-c4d6-4ff3-84bf-fdb3eb0d6845
  modified: 2026-10-30T14:44:25.951Z
---

下流のセッションに渡す物(`data/derived/.../kgb_input/` など、`_meta.tool.commit` を持つ物)は、道具をコミットしてから書き出す。直しの確かめは、関数を読み込んでメモリの上で行い、本物の置き場には書かない。

**Why:** 2026-10-30、dcr が S2 の道具をコミットする前に、「出力が前と同じか」を確かめるために本物の置き場へ書き出した。その結果、`_meta.json` の `tool.commit` が、実際に書いたコードの 1 つ前のコミットを指した(要素は同じ)。kgb と coord が、`_meta` の欄と版の食い違いから見つけた。`git_head()` は HEAD しか見ないので、気づけない。

**How to apply:** 書き出しの道具には「未コミットの変更があれば何も書かずに止める」守りを入れる(dcr の `export_kgb_input.py` は a7e229c で入れた・利用者承認)。新しい書き出しの道具を作るときも同じ。読み手の道具の `tool_dirty` の印は、止めない流儀なので、下流に渡す物には足りない。関連: [[remerge-main-before-writing-comms]]
