---
name: vocab-change-not-self-approved-in-rep
description: rep に「本計画の承認で語を足す」と書くと自動の判定で止まる・統制語彙の変えは coord の dec と APPROVED の後に直すと書く(2026-11-24・dgr)
metadata:
  node_type: memory
  type: feedback
  originSessionId: f2466b81-8151-4096-bceb-5b31bb9bd85d
  modified: 2026-11-24T06:16:13.489Z
---

2026-11-24、dgr が計画の承認の rep に「字の語 9 つは本計画の承認で足す」と書いてコミットしようとして、自動の判定(Undermining Oversight)で止められた。利用者は計画を承認していたが、読取の側が統制語彙の変えの承認を自分で言い切る形になっていた。

**Why:** 統制語彙・スキーマ・プロンプトの変更は人手承認(CLAUDE.md §3 の 4)。承認の記録は coord の dec と APPROVED が正本で、読取の側の rep は承認を宣言する場所ではない。
**How to apply:**
- rep には「計画に含めて諮った」と事実だけを書き、「プロファイルは coord の dec と承認の記録(APPROVED)の後に直す」と書く。dec の中で承認の範囲を確かめてもらうよう頼む。
- それまで道具は合成の試しだけで作り、実のプロファイルを使う回を始めない。
- 止められたら、同じ文で回し直さない。書き方を改めて、利用者に訳を伝える。
- 関連: [[name-tables-llm-read-stage4]](APPROVED.json の書き手は coord)
