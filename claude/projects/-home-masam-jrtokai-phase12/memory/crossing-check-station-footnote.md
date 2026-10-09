---
name: crossing-check-station-footnote
description: 踏切質問では踏切制御図表だけでなく管轄駅の連動図表の脚注(備考)も必ず確認(利用者重要指示 2026-08-10)
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-10T01:53:16.767Z
---

**踏切に関する質問(支障報知→停止信号 等)では、踏切制御図表の備考だけでなく、その踏切を管轄する駅の連動図表の脚注(備考)も併せて確認する**(利用者重要指示)。

**Why:** interlocking-006(津街道第一踏切の支障報知→停止信号)で、踏切制御図表の備考には記載がなく、管轄駅=亀山駅連動図表の脚注3項目(第二踏切は4項目)に全リストが記載されていた。踏切単体の図だけでは情報が欠ける。

**How to apply:** (1)新しい踏切/駅の抽出時は連動図表の脚注(備考)をpdftext/visionで取得し、支障報知→停止信号は HAZARD_STOPS として加法投入(再現=`knowledge_kb_v8/scripts/ingest_kameyama_hazard.py` が雛形。亀山は投入済・一部信号ノード未収録分は再実行で回収)。(2)脚注の他項目(一斉停止現示てこ等)も注記として構造化候補。(3)回答生成では管轄駅の脚注由来データも参照させる。関連: [[all-targets-extraction-principle]] [[clearance-x-boundary-turnout]]。
**追記(2026-08-10)**: 『(駅名)構内(踏切名)踏切』(例 富田構内八幡踏切)=その駅が管轄する踏切の一般表現。照合は駅名・『構内』を除いた核(八幡踏切)で行う(SCHEMA_HINTSに規則化済)。桑名の固有名『構内踏切』とは区別。
