---
name: singularity-display-plan
description: D-KB 特異点(alert・singularities)の表示側(fed)— 決90/決92 で承認・実装済み(1cfda730)・反映と旗 ON は別承認
metadata:
  type: project
---
計画 docs/計画_D-KB特異点の表示側_fed_20260925.md(改訂 1・2fbd9bb24f4ff367)を決90(2026-09-25)で承認。実装 e355bb79・rep 20260925-1935。
- 配る物: Dify dkg_diagnose.py 4b8748588b10de7d(0.3.9)・demo_shirei f2d8e36ee624eb3c・demo_v10 0c101046a0a65ac3。3 か所に同じ部品(SG_*・sg_*)を写し、試験で構文木の同一を確かめる。
- 試験: knowledge_kb_v8/scripts/fed/test_singularity_display.py 34(dkb の見本 16 本は本体ツリーの git 外に在る。無ければ FAIL)・kg_api/demo_shirei/test_ui_singularity.py 62。手順書 §8-7。
- Streamlit の st.warning は本文の先頭の絵文字を外す → 記号は icon= で渡す。
- 決92(2026-09-25): EC2 反映を承認(coord が実施)・旗 ON は反映を確かめてから coord が入れる。too_many_groups の括弧は規8 の札にそろえた(1cfda730・新 sha16 は dkg_diagnose 405721a5・shirei 151848e7・v10 34908d49)。「理由の記録なし」は承認済み。
**Why:** 旗 ON を先にすると、demo_shirei ① が名前札の空いた alert を出す。
**How to apply:** 順は、反映(プラグインは uninstall→install・画面は作り直し)→ 旗 ON。どちらも別承認。関連 [[safety-mark-display]] [[dify-plugin-update]]

**決133(2026-09-28 14:36)**: shirei/v10 のサンプル 58 問(S1〜S6)に「駅名+NN号」が 0 問で alert はサンプルから出ない → S7 = 特異点の 3 問(新城 34 号 転換不能 strong・高山 12 号 故障 medium・新城 34 号 不転換 strong・文は info 20260925-2005 のまま)を fed が加法で足す(req 1436)。EC2 反映(shirei 151848e7・v10 34908d49 の置換)は coord。

**決133 反映済み(2026-09-28 14:4x)**: fed 5eb5b9b2(shirei 58→61・v10 50→53・test_demo_samples_s7 PASS)→ coord が EC2 へ置換(shirei 14a040bd・v10 1642a5f3・タグ pre-s7-20260928/s7-20260928・health 200)。残り = 利用者の shirei での S7 目視。

**所有者の注意(2026-09-29 確認)**: 表示側 3 つ(`deploy/dify/plugin/*`・`kg_api/demo_shirei/*`・`kg_api/demo_v10/*`)は OWNERS 40〜42 行で **fed の所有**。0.3.9 の実装(e355bb79・1cfda730)も fed のコミット。計画は dkb でも、ファイルを直すのは fed。決139(instance_only の文言・0.3.10)で coord が「dkb の所有」と誤って振ったので、dkb は実装せず rep 0043 で返した。**How to apply:** 表示側を直すよう頼まれたら、まず OWNERS の現物を見る。
