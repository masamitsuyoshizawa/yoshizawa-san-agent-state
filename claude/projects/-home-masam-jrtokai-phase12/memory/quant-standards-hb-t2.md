---
name: quant-standards-hb-t2
description: 数値基準の整備状況 — HB定量化523件・定格値72件抽出(投入承認待ち)・Ⅴ編取替目安+T2規範値44件格納済
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-03T11:49:30.641Z
---

数値基準整備の到達点(2026-09-03):
- HB standardの定量化: 455→523件(抽出漏れ81件のうち68件をハイブリッド方式=機械窓検出+LLM原文引用+span検証+op決定論で再構造化)。残57件は表パーサ課題。真の定量化候補(原文に数値なし)=102件は顧客提示対象(docs/提案_HB検査基準の定量化候補_20260903.md v3)。
- 定格値: 全依拠文書走査で72件抽出済み(docs/定格値抽出_依拠文書調査_20260903.md)。**KB投入はスキーマ承認待ち**。定格参照型standard45件のうち11件(電源切替器・整流器・発電機系)は台帳待ち。
- HB Ⅴ編「取替の目安」: ReplacementGuideline 4件(リレー線区別10-30年・80万回根拠)+契機事故CaseStudy2件を格納。診断はリレー×接点/経年系候補時のみ replacement_guidelines_HB を併記。
- T2規範値44件: 正本 kg_api/kb/data/dkb/normative_standards.json(ns:NNNN)・MeasurementStandardラベル共有・prov_doc='T2'。検査提示は追補枠(既存topm不動+ns1件末尾)。
- EC2反映済(docker commit jrtokai-v9-kg-api:v5t2-20260903)。

**Why:** 台帳が当面得られない中、依拠文書内の数値基準・定格値を最大限回収する路線。機械regex単独は偽陽性29-45%で不可、ハイブリッド2段(緩規則検出→LLM窓照合→span検証)が確立方式。
**How to apply:** 次回LOO時はjudge KG_SCOPE較正(Ⅴ編・T2規範値追加を反映)が必要=[[judge-scope-calibration]]。基準抽出の再走査時は「定格」以外の基準示唆語27種(extract_normative_t2.py のTERMS)を使う。関連=[[dkg-cause-kg]]。
