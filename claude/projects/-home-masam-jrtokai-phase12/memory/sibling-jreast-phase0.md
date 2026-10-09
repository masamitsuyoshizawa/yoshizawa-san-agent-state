---
name: sibling-jreast-phase0
description: 姉妹プロジェクト ~/jreast-phase0(JR東日本DWG連動図表→QAデモ)を本成果の片方向コピーで立ち上げ済
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-07-29T04:17:59.131Z
---

姉妹プロジェクト **`~/jreast-phase0`**(別会社=JR東日本の連動図表 DWG を構造化・グラフ化して QA するデモ試作)を 2026-07-29 に立ち上げた。**別 Claude Code セッション(cwd=~/jreast-phase0、メモリ名前空間 `-home-masam-jreast-phase0`)で運用**する(jrtokai-phase12 とは分離)。

- 再利用は **jrtokai→jreast の片方向コピーのみ**(自動 cross-import 禁止=結合・他社データ逆流の防止)。移植済=`engine/`(neo4j_client・llm_providers・prompt_manager・nl_pipeline・reference_kg_api_app)。SCHEMA_HINTS/glossary/統制語彙/プロンプト/抽出は JR東日本向けに作り直す。
- 提供データ `~/jreast-phase0/provided_data`(2.9GB, DWG610/PDF1333/xlsx57、各支社の駅別連動)= 他社専有・**絶対非コミット**(.gitignore済・git add -A禁止)。
- DWG版 AC1032(2018)含む → DWG→DXFは **ODA File Converter 推奨**(LibreDWGは2018非対応)。ezdxf 1.4.4 で DXF解析。
- 立ち上げ記録=`~/jreast-phase0/docs/セットアップと初期手順_20260729.md`。次段=DWG→DXF変換PoC→レイヤ/記号/連動表レイアウトの棚卸し→抽出設計。

本プロジェクト(jrtokai)の [[eval-and-table-comprehension]] の表構造化手法・[[abc-unified-api]] のkg_api が主な再利用資産。
