---
name: pipeline-change-regen-contract-codes
description: ingest_pipeline.py など工程のソース 4 本を変えたら gen_intake_pipeline_codes.py --write で契約の生成区画を書き直し、契約の試験も回帰に入れる
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-10T03:08:07.591Z
---

工程のソース 4 本(`accident_kb_v7/scripts/ingest_pipeline.py`・`ingest_bundle.py`・`llm_usage.py`・`ingest_one.py`)を変えたら、同じ便で `kg_api/kb/scripts/gen_intake_pipeline_codes.py --check` → `--write` を回す。`kg_api/sources/intake_contract.py` の生成区画(札の一覧と `PIPELINE_SOURCES_SHA16`)が書き直され、fed の契約の試験 C9 がそれを照らす。

**Why:** 2026-11-10 の決352 追補 21 で ingest_pipeline.py に欄を足したとき、生成区画を書き直さず、回帰の 42 本にも契約の試験を入れていなかった。札は同じでも sha16 の行だけ外れ、fed が見つけた(info 20261110-1207)。生成区画は bkb の道具の持ち分なので、他のセッションは書き換えない。

**How to apply:** 工程のソースを触る変更の手順に「生成区画の --check / --write」と「契約の試験」を入れる。回帰の一覧を作るときは、変えたファイルを読み込む試験だけでなく、そのファイルの sha を固定している所(生成区画・manifest・台帳)も探す。[[bulk-intake-r352-bkb]]
