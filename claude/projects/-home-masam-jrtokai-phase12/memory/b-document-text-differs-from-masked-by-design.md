---
name: b-document-text-differs-from-masked-by-design
description: B 正本の Document.text は data/masked と 348 事故・1,392 か所で違うのが正常(09-25 の承認済みの地名の是正)・masked が是正の前の版
metadata:
  type: project
---

B 正本(と EC2・10/22 の顧客 dump)の `Document.text` は、`accident_kb_v7/data/masked/*.txt` と **348 事故・1,392 か所**で違う。これは 2026-09-25 の**地名への誤ったマスクの是正**(決86 (4)・決88・決91・決94・決95・dkb の `unmask_fields.py`・ingest_pipeline 積荷 unmask G16「文書 1392 か所」)で `[氏名]` を地名へ戻した結果。決88 諮2 で「置き換える前の値を正本に残さない・戻す道は投入の前の書き出しと data/masked」としたので、**masked ファイルは意図して是正の前の版**(sha16 が是正の前の Document と 348/348 一致)。

2026-10-27 に bkb がこれを「Document.text が古いマスクのまま」と逆向きに読み、決313(個人情報・最優先)が出た。dkb が工程の記録と位置の表で照合して訂正(rep 20261027-0514)。

**Why:** 差の向きを日付(ファイルの更新日)から推すと逆になる。是正の記録は pipeline_state と unmask_fields にある。
**How to apply:** Document.text と masked の差を見たら、先に `unmask_fields.py --check-rewind`(読み取り)と位置の表の件数で照らす。`build_fulltext.py` を全件で回すと是正が巻き戻る(決263-A 追補 5)。決312 の索引を masked から作ると地名が [氏名] のまま。関連 [[dkb-coverage-singularity-bkb]]・[[marker-in-source-breaks-alignment]]。


(索引から移した説明・2026-11-07)
- [B の Document.text と masked の差は正常](b-document-text-differs-from-masked-by-design.md) — 348 事故・1,392 か所は 09-25 の承認済み地名是正
