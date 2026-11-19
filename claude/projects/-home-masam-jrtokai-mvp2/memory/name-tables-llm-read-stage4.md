---
name: name-tables-llm-read-stage4
description: 決161 の段 4 の改め: 名称表 9 頁を 3 種の CLI で書き写す(vol_read.py)・APPROVED.json の段の書き込みは自動の判定で止められた(2026-11-20)
metadata:
  type: project
---

名称表 9 頁(2 冊・H0 合格)は、字の層が崩れていた(記号の欄の形に合う割合 約 69%・手元の OCR は約 96%・2 つの一致 約 36%)。利用者は 2026-11-19 に dgr の画面で「LLM 3 種で読み、照らす」を選び、Plan Mode で計画とプロンプト・JSON の型を承認した(決161 の「AI に読ませない」を改める)。9 頁は記号と名称の表 5 頁と図面の目次 4 頁で、記号の絵の列は無い(決159 の rep の推定は外れ)。

- 道具: scripts/drawing/vol_read.py(cf7951f)・prompts/vol_table_transcribe_v1.md(sha ce6752fc…)・schemas/vol_table_transcribe_v1.json(sha 2362ac13…)。切り方は横の罫線の切れ端(40 画素・覆い 25%)で表の上下を決め、罫線の上で切る(58 枚)。
- **APPROVED.json に段 vol_tables_cli を dgr が書こうとしたら、Claude Code の自動の判定が「監督を損なう」として止めた。** 回り道はせず、利用者に書き方(coord が dec と共に書く・推し / 利用者が ! で書く / 許す)を問うている。門は段が無いと何も送らない(selftest 14/15 の残り 1 がこれ)。
- 改めは rep 20261120-0131 → **決165(2026-11-20 01:33)** に記録された。coord は段を代わりに書かない(止められた書き込みの代行は、利用者の許しの決めを回り込むため)。利用者が書き手を指示するまで、実も合成も送らない。merge の補助(字の層と OCR との一致)は 8938f62 で足した(試し 15/16)。
- 次: 段の記入 → 合成の 1 枚を 3 種へ → prepare → 試しの 1 頁(80 頁の冊の 78)→ rep → 記号の欄の 3/3 が 8 割以上で coord が見たら残りへ。

**Why:** 承認の記録は人の監督の物なので、作り手が自分のプロンプトの承認を書くのは止められる。
**How to apply:** 承認の記録(APPROVED.json の段・dec)は、coord か利用者が書く形で頼む。[[review-ui-common-parts]] [[symbol-inventory-jis-dec143-150]]
