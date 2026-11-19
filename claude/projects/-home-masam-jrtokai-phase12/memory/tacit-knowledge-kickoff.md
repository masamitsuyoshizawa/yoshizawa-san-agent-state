---
name: tacit-knowledge-kickoff
description: 暗黙知対応(並行ジョブ・2026-10-29)= 指令経験者への聞き取り JSON を KB に取り込む検討・サンプルの置き場は ~/jrtokai-tacitknwoledge(綴り違い)・別 DB 推し・利用者判断 D1〜D6 待ち
metadata:
  type: project
---

2026-10-29 に利用者が新しい並行ジョブ「暗黙知対応」を依頼。JR 東海が事故摘録をもとに指令担当経験者へ会話形式で「なぜその作業・判断をしたか」を聞き取っている。その記録を KB に取り入れ、原因診断と作業指示に活かす。条件 = 既存の API 機能は変えない。

- サンプルの置き場 = `~/jrtokai-tacitknwoledge/暗黙知抽出会話履歴20261029.json`(依頼文は tacitknowledge・実物は knwoledge)・sha16 b1207b742c7852af・会話 1 本 22 発話。**role: user が経験者・assistant が聞き手の LLM**(知識は user 側だけ)。欄に話し手の氏名・利用者 ID(格納しない)。
- theme の原本名が census に完全一致 → acc:0055。D-KG はマニュアル由来で事故 ID を持たず、VWOUT・FS32 の CauseConcept・FieldCheck が既に在る。
- coord の検討書 = `docs/検討_暗黙知対応_事故摘録への紐付けと格納と活用_coord_20261029.md`(改訂 0)。推し = 別の DB「暗黙知 KB」・紐付け 3 層(事故 / 経過の行 / 知識)・加法の口 /v1/tacit/*(旗既定 0)・診断の出力は第 1 段では変えない。
- 利用者の判断待ち D1〜D6(格納の場所・知見の型の統制語彙・LLM へ送る範囲・顧客側の様式・担当〔新セッション tkb 案〕・第 2 段の評価)。

**Why:** 経験者談はマニュアル由来・事故摘録由来と出典の種類が違い、混ぜると診断の根拠が区別できなくなる。B/D-KG に入れると既存の digest と応答が変わる。
**How to apply:** 判断が出るまでデータに触れない。docs には会話の原文・氏名・原本名を書かない(書いたら 10 字以上の一致を機械で照らす)。関連 [[mention-facts-belong-on-edges]]・[[prefers-stratified-sampling]]。

**決337(2026-10-30 00:15・利用者決定)**: D1 = 別の DB「暗黙知 KB」(計画値 neo4j-tacit-v1・bolt 10290 / http 10277)・D2 = 9 型を初版として承認・D3 = マスク後の経験者の発話だけを LLM へ・D5 = 新セッション **tkb**(作業ツリー `../jrtokai-phase12-tkb`・枝 tkb・起動の文 `docs/起動プロンプト_tkb_暗黙知対応_20261030.md`)。残り = D4(顧客側の様式・利用者が顧客と)・D6(第 2 段は層化 20〜30 件の後に LOO)。原本の置き場 = 本体ツリー `accident_kb_v7/data/tacit/raw/<sha16>.json`(700/444・サンプル配置済み)・仮名の鍵 `…/tacit/private/`。tkb の第 0 巡 = 設計書(実装しない・承認の後に実装)。連絡箱の道具の略号の一覧(comms_dispatch / comms_unread)に tkb を足した。

**tkb 起動と第 0 巡(2026-10-30 00:52)**: 設計書 `docs/設計_暗黙知KB_tkb_20261030.md` 改訂 0(78c46b3af3bd471b)。L1 は census だけで acc:0055。**coord の検討書の見落とし** = 既存の Phase E「暗黙知 DB」(`knowledge_kb_v8/scripts/build_tacit_kb.py`・事故の対応履歴から 130 件・federate の `kind="tacit"`)が実在 → 本件は `source_kind: "interview"`(経験者談)で分け、Phase E は触らない。tkb への連絡は SendMessage(宛先 jrtokai-phase12-tkb)・tkb からは jrtokai-phase12-coord へ(利用者の指示で送り方を伝え済み)。
**決339(2026-10-30 01:16・利用者決定)**: U2 = 規則マスクの後に NER + 人の氏名の門 / U3 = 整形の 3 欄は返し逐語は位置だけ / U4 = 対応表を持たない(鍵だけ・gpg 退避)/ U5 = **Opus 5.5 jp(bedrock-opus-5-5・国内の地域・temperature 無し → 記録と再生で固定)**(tkb の推し opus-4-7 ではなく利用者が選んだ)。残り = U1 プロンプト・U6 スキーマ(rev の点検の後)・D4・D6。


(索引から移した説明・2026-11-07)
- [暗黙知対応(並行ジョブ)](tacit-knowledge-kickoff.md) — 決337 = 別 DB・9 型・マスク後の経験者の発話だけ・新セッション tkb(2026-10-30)・置き場は本体ツリー accident_kb_v7/data/tacit/
