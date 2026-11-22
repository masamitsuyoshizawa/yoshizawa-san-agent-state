---
name: no-ui-edits-while-serving
description: 確かめの画面を配っている間は、部品(scripts/review_ui の ui.js・ui.css)を木の上で書き換えない・部品は問いを開くたびに画面の字を読み直していた(2026-11-22・dgr・coord が急ぎで止めた)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-21T18:09:52.812Z
---

配っている確かめの画面(rui.serve・rui.App)は、問いを開くたびに ui.js と ui.css をファイルから読み直す作りだった(core._asset)。dgr が 8753・8756 を配ったまま、木の上で作り途中の部品(0.9.11)を書き換え、利用者に案内する前の画面に、数えも通しも済んでいない字が入った。coord が画面の字の sha16 を照らして見つけ、急ぎで戻させた(答えは 0 で、害は無かった)。

**Why:** 利用者が答えている最中に、確かめていない字で保存や表示が崩れうる(同じ日の、備考が消えた件と同じ種類の危うさ)。memory の「送りの最中は道具の置き場を書き換えない」の、画面の部品の版でもある。

**How to apply:**
- 配っている間は、scripts/review_ui/ を木の上で書き換えない。作り途中は、別の worktree か、木の外の置き場で進める(作り途中の写しは data/derived/_dgr_dev/review_ui_0911_wip/)。
- 部品を直して、App を作る時に字を読み込んで持つ形にする。起動の時の字の sha16 を、答えの記録と配りの知らせに書く(coord が main の版と照らす)。
- acc_review_server.py は、回の置き場の index.html を配るだけなので、部品の書き換えでは変わらない(回の置き場を書き直した時だけ変わる)。
- [[no-tool-edits-while-runs-record-version]] [[review-ui-common-parts]] [[review-screen-approve-only-with-function-checks]]
