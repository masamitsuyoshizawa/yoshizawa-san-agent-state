---
name: mechanism-text-list-repr-pending
description: 機序の文の章節がリスト表記(D-KG prov_ref 319 件)・決292 はプラグイン側で是正・dkb 案 A は残課題(2026-10-22)
metadata:
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-22T13:44:35.435Z
---

dkg_backend.py 137 行の候補の機序の文 = `名 + '(' + prov_doc + ' ' + prov_ref + ')'`。D-KG の prov_ref のうち 319 件(prov_ref を持つ 9,450 件中)が章節を Python のリスト表記で持つ(`p.299 ['第２編 信号設備', '第６章', …]`・改行 0)。字数で切る読み手(プラグイン 120・dkg-review 130・demo_v10 160・federate_router 80)で閉じの「]」が落ち、Dify が書きかけのリンクと読む。E2 base 1,345 中 201 応答・421 文。

決292(2026-10-22 22:43)は fed のプラグイン 0.3.18 だけで是正。dkb 案 A(照会の後に文字列だけのリスト表記を「a > b > c」の 1 行へ・D-KG は変えない)は**別件の残課題**(dec 20261022-2243 追補 1)。

**Why:** mechanisms の文が変わると基準の写しと受入、federate の LLM の入力がやり直しになるので、ボタン回答の便に混ぜなかった。
**How to apply:** 利用者に諮られたら rep 20261022-2242 の案 A から始める。demo_v10(st.caption = Markdown)は 160 字で同じ壊れ方をしうるので、そちらの不具合の報告が来たらこの件に結ぶ。関連 [[dkg-cause-kg]]
