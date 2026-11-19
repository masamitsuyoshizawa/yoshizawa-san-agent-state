---
name: sat-strategy-is-for-other-customer
description: SAT の戦略(LLM・Copilot への対抗)の検討は JR東海向けではなく、別の顧客へ SAT の価値を説明するため・本 PJ の開発に影響させない(利用者指示 2026-11-06)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-11-06T11:10:01.889Z
---

2026-11-06 に利用者の依頼で、SAT の戦略を coord・pla・agy の 3 案で検討し、取りまとめた。
- 文書: `docs/取りまとめ_SATの戦略_coord_pla_agy_20261106.md`(sha16 032df0bc954fa208・改訂 0)。推しは「業務判断の保証の層」。
- 利用者の訂正(同日): この検討は JR東海向けではない。顧客(非 JR)に「LLM と併存する SAT の価値」と「今後の方向性」を説明するための検討の一環。
- 求める成果: 顧客向けの説明。SAT の価値・Copilot に代表される AI モデルとの違い・今後の戦略を、納得性高く訴求するもの。

- 成果(11-06): 顧客向け資料 `/home/masam/sat-value-briefing/`(git 外・本 PJ のリポジトリの外)= SMテンプレの pptx 12 枚・経営と事業部門向け・事例は匿名と定性だけ・メモあり sha16 66ed7bd8e8c49eb1 / メモなし d3aa84f491475319(初版)・build_deck.py で作り直せる。
- 3 枚版(11-06・利用者の指示): 表紙・p6(二層)・p10(実証)を省き 3 枚に集約 = build_deck3.py・メモあり sha16 f17302c2d59f2c7e / メモなし d9bb41ad8d739087。
- 3 枚版の改訂(11-06・利用者の指示): 2 枚目の状況の欄と「実証済み・これから」の印を外し、API・MCP は ③ の題と文・図の矢印の札で示す = メモあり 87931e19a192a721 / メモなし ea6ebb03b5d3bd53。3 枚目の状況の印も外した・3 枚目の「今・次・その先」も消して 3 段の箱を低くした(同日・最新 メモあり ab3a30441100d080 / メモなし ce7260aded3f93c2)。スライド上は状況の印なし・今の機能かの区分は発表者メモの社内確認だけ。

**Why:** 本 PJ(JR東海)の開発の方向を、この戦略の議論で変えてはならない。

**How to apply:**
- この話題では、fed・bkb・dkb などへ req・dec を出さない。本 PJ のコード・設計・計画に結び付けない。
- 顧客向けの資料に JR東海 PJ の名前・数値・事例を使うかは、利用者の判断。勝手に載せない。
- 関連: [[mvp1-final-report-slides]] [[agent-newcomer-briefing-deck-20261105]]


(索引から移した説明・2026-11-07)
- [SAT の戦略は別顧客への説明用](sat-strategy-is-for-other-customer.md) — JR東海向けではない・本 PJ の開発に影響させない・顧客向けに SAT の価値と Copilot との違いと今後を訴求(11-06)
