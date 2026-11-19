---
name: diagram-dynfw-phase0-kickoff
description: 新タスク「図表解析の動的フレームワーク」を別リポジトリ ~/diagram-dynfw-phase0 で開始準備(2026-10-21)・目録参照のみ・連絡箱方式を移植・第 0 巡の req 3 通
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-10-21T06:29:14.555Z
---

2026-10-21 に利用者の依頼で、顧客非依存の新タスク「図表解析の動的フレームワーク(サブセットを多数そろえ課題ごとに抽出・組み合わせる)」の開始準備を `~/diagram-dynfw-phase0`(新規 git・main・初回コミット 830eab1)に作った。

- 利用者判断(2026-10-21): 置き場は新規リポジトリ(GitHub origin は利用者が後で作成)/ 本 PJ のノウハウは**目録と参照パスだけ**(`docs/01_ノウハウ目録_jrtokai由来_20261021.md`・札 G/M/C・実物は漏洩点検後に移植)/ 体制は本 PJ の連絡箱方式を移植。
- 体制: coord(Claude Code・main)・icc(Claude Code・案出し)・icx(Codex・案出し)・iag(Antigravity・案出し)・rev(Codex・批判)。worktree `../diagram-dynfw-phase0-{icc,icx,iag,rev}` 作成済み。AGENTS.md は §A icx / §B iag / §C rev。**案出し 3 者は互いの案を見ない**(coord も渡さない)。
- 文書: `docs/00_ミッション…`(§7 検討の問い 8・§11 第 0 巡の依頼)・`03_進め方と体制`・`04_サブセット様式_v0`・`05_漏洩点検_方針v0`・`06_起動プロンプト_各セッション`。第 0 巡の req は `docs/comms/20261021-1528_coord_to_{icc,icx,iag}_req_round0-concept-proposal.md`(期限の目安 10/23)。
- 道具: `scripts/comms_*.py` は本 PJ の複製で、セッション名と接頭辞 `diagram-dynfw-phase0-` に差し替え済み(説明文に本 PJ の出来事が残る)。dec の番号は `決1` から(本 PJ の決N とは別系列)。

**Why:** 顧客固有リポジトリ(jrtokai-phase12)と分けて漏洩を防ぎ、独立の案出しを構造で担保するため。
**How to apply:** 新タスクの作業は新リポジトリの coord セッションで行う。本 PJ の連絡箱には書かない。本 PJ から持ち込むときは目録 → 札 → 漏洩点検 → 一般化 → `subsets/` の順。関連 [[next-phase-challenges-ab]]・[[skill-consolidation-file]]・[[session-comms-protocol]]。
