---
name: page-label-convention
description: 出典ページ表記規約(印字pNN/PDF通しpNN/スライドN)を制定・全表示層へ適用済み(2026-09-02承認)
metadata: 
  node_type: memory
  type: project
  originSessionId: cdd0397a-bba7-471e-80ab-cd200525a491
  modified: 2026-09-02T03:29:19.563Z
---

出典ページ表記規約(2026-09-02利用者承認): 紙面印字番号=`印字pNN`(最優先)、印字不明=`PDF通しpNN`、pptx由来=`スライドN`、xlsx由来(M1異常時対応事例集)=`事例N`/`シート『名』(事例N)`。M1のA索引page_startはシート通番でない(通しチャンク番号)ため使用禁止・case_idが正本ロケータ。M2はevent_id=スライド番号。D-KG Checklist/ResponseFlowノードに prov_sheet/case_id/prov_slide/slide_no を加法SET済(EC2+ローカル両方)。

- T1/T2はフッター「ー N ー」から物理→印字対応表を決定論生成(`knowledge_kb_v8/scripts/build_page_map.py`→`kg_api/kb/index/page_map.json`、T1=370/385p・T2=509/523p・逆行0)。HB=章節別コードフッター・T3=画像PDFのため対応表なし(PDF通しp)。
- refs索引chunks.jsonlへ`page_print`を加法付与(1867/2030件、`add_page_print_refs.py`)。F/R系旧docxチャンク163件は実体が目次断片でvision突合不成立→付与見送り(創作禁止)。
- 表示層: app.py refs注入ヘッダ・docs_backend.search page・federation.py citation9箇所(`_page_label`)・split_main_sub出典規約(`patch_pagelabel_20260902.py`)。
- EC2はkb/index・dkb・refs_c_indexとも**roマウント**: データ更新はホスト側`/home/ubuntu/jrtokai-v9abc/{index,dkb,refs_c_index}`へsudo cp→コンテナ再起動。
- 関連: [[eval-and-table-comprehension]] [[skill-consolidation-file]]
