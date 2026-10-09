---
name: approval-line-for-own-artifacts
description: 自分の所管の成果物に主張を減らさない守り(検査)を足すのは承認不要。承認が要る 4 類型
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7d23b295-de1f-476e-9436-0c42a7039f2a
  modified: 2026-09-18T11:18:32.143Z
---

coord が 2026-09-18 に引いた線。**自分の所管の成果物に、主張を減らさない守り(常設検査など)を足すのは、承認を待たずにやってよい。申告だけで十分。**

承認が要るのは次の 4 つ。

1. 統制語彙・スキーマ・プロンプトの変更
2. 他セッション所有のファイルへの変更
3. **既存の検査の主張を減らす変更**
4. 共有資源(B/C 正本グラフ・EC2)への書き込み

**Why:** CLAUDE.md の「実装の前に必ず計画を提示し承認を得る」を、守りを足すことにまで広く当てていたため、承認待ちで止めていた。実際に問題になるのは上の 4 類型で、検査の追加はそこに当たらない。

**How to apply:** 自分の所管ファイルへ検査を足すときは、実施してから報告する(変異で空振りでないことを確かめたうえで)。主張を **減らす** ときだけ承認を取る。[[artifact-self-report-not-trusted]] [[negative-test-check-reason]] [[ingest-effect-unmeasured]]
