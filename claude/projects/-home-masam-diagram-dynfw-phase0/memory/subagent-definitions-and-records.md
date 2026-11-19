---
name: subagent-definitions-and-records
description: .claude/agents/ に置いた定義は起動中のセッションでは使えない。補助のエージェントの記録は subagents/ の JSONL に残る(2026-10-24 確認)
metadata:
  node_type: memory
  type: reference
  originSessionId: fdc4c948-da33-4847-b9a6-04212d37500d
  modified: 2026-10-24T06:38:56.075Z
---

- `.claude/agents/<name>.md` を起動中のセッションで新しく置くと、Agent の subagent_type には出ない(`Agent type ... not found`)。使うには coord のセッションを始め直す(2026-10-24・CLI 2.1.307 で確認)。
- 前付けには、注記であっても `---` の文字を書かない。CLI の読み方の 1 つ `^---\s*\n([\s\S]*?)---\s*\n?` は、行の途中の `---` でも前付けを切り、定義が黙って読まれなくなる(2026-10-24・2.1.310 で起きた・決68)。置く前に同じ正規表現で前付けが最後まで読めるかを確かめる。
- 定義の前付けの欄(CLI 2.1.307 の欄の説明で確認): `tools`・`model`(別名か完全な ID)・`maxTurns`(API の往復の上限)・`effort`・`omitClaudeMd`・`background`・`memory` など。
- 補助のエージェントの記録は、プロジェクトのセッションの下の `subagents/agent-<ID>.jsonl` と `.meta.json` に残る。行の種類は `assistant`・`user`・`api-request`(要求 1 回に 1 行)・`api-request-shape`・`api-request-blob`・`attachment`。背景の起動では、応答は `SubagentHandback` の `input.message` で返る。記録には画像のデータとシステムの文が入るので、リポジトリに入れない。

**How to apply:** 試作 B の観測の担当の定義を置いたら、送信の前にセッションの始め直しを利用者に頼む。応答と使用量は記録から機械で取り出す(書き写さない)。[[delegate-mechanical-checks-to-ai]]
