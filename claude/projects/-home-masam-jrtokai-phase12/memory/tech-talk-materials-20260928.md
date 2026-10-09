---
name: tech-talk-materials-20260928
description: 技術説明会(項目 1 システム全体アーキテクチャ・項目 3 KG 構築)の基礎資料 2 本と Slides Artifact 31 枚を 2026-09-28 に作成。作成者が編集する前提・API の中身に比重
metadata:
  type: project
---

利用者の依頼(2026-09-28): 技術説明会アジェンダの 1(システム全体構成・構成要素・分割理由)と 3(KG の概要と分割理由・データ収集と選定・ルール/AI 抽出・Entity/Relation 抽出)の基礎資料。WebUI/エージェント層は一般形で、API の中身に比重。項目 2(エージェント設計)は作成者側。

- 詳細文章: `docs/技術説明_基礎資料_1_システム全体アーキテクチャ_20260928.md`(a730845095ba3d1e・245 行)・`docs/技術説明_基礎資料_3_ナレッジグラフ構築_20260928.md`(8dc7c1749f18a39e・212 行)。
- スライド: Slides Artifact https://claude.ai/artifact/QuGJLddFTUPCTyXDAPf7qt(31 枚: 表紙・前提・項目 1 ×13・項目 3 節見出し + ×14・まとめ)。ソースは `docs/技術説明_スライド_20260928/`(deck.json + slides/*.html)。pptx/PDF は Share › Export。
- 事実の出どころ: Explore 4 本(全体構成・B・C・D/A)+ 直接読み(API仕様_全機能・V7 設計確定メモ・D-KG 技術詳説)。数値は取得日つき(B 27,267/46,793・C 8,055/16,585・D 14,467/24,534・語彙 7,840/9,549・A 3,906)。
- 注意点: D の「Mechanism 140」は T2 分のみで全体は 340。「647 事象」は評価集合であってノード数ではない。B の 9677 ポートは存在しない(B は 9890 のみ)。facet_router の docstring「未登録」は古い(app.py で登録済み)。

**Why:** 説明会の作成者が API の中身を正確に語れるように、コードと設計文書から起こした事実集。
**How to apply:** 数値を更新するときは取得日を併記し、判定器の版を混ぜない。関連 [[agent-docs-bkb-dkb-20260928]] [[facets-instance-entry-issue]]

**項目 3 の PPTX(2026-09-28 17:5x)**: `docs/技術説明_項目3_ナレッジグラフの構築方法_20260928.pptx`(+pdf)。docs/SMテンプレ.pptx(10×5.63in・レイアウト 3 種: 表紙/INDEX/白地本文・テーマ色 1966B3/00937E/FF5A4A・表紙のプレースホルダは 2 つとも idx 0 なので位置で判別)の上に python-pptx で組んだ。表紙・目次 + 7 枚(全体像/データ収集/ルール抽出/図表読取/B 構築/D 構築/Entity-Relation と語彙・品質)。生成スクリプト docs/技術説明_スライド_20260928/build_pptx_item3.py。描画確認は soffice → pdftoppm。

**v2(18:1x)**: `docs/技術説明_項目3_ナレッジグラフの構築方法_20260928_v2.pptx`。利用者の指示 = 題「3. ナレッジグラフの構築方法」・項番 3-N・出典行なし・色分けの統一(v1 の 3-3 で人手と決定論の色が流れ図と凡例で逆だった)・平易な言い換え。色の規約: AI/LLM = コーラル FF5A4A・決定論 = テール 00937E・人手 = 青 1966B3、各ページ右下に凡例。v1 は残す。生成 build_pptx_item3_v2.py。

## 追記(2026-10-06)
- ckb の点検 2 回(rep 0245・1118・1129)で 12 件 + 4 件 + 軽微 2 件を訂正(判定器は Opus 5・enrich_lookup は LLM 1 回・用語集 39/103・118/0/53 の母数・7,726 は値の件数)。Slides Artifact 版 9。**a05 は利用者が Slides 編集器で触った版が公開されていたので、公開版に訂正を当ててローカルも同期**(ローカルを上書き publish しない)。
- 項目 1 の PPTX: `docs/技術説明_項目1_システム全体アーキテクチャ_20261006.pptx`(build_pptx_item1.py・雛形 SMテンプレ.pptx・利用者指示 = 「口」→「エンドポイント」・docs/search と eval/judge を外し rawdata と doc を加える)。
- API リファレンス(全 37 エンドポイント): `docs/API_リファレンス_全エンドポイント_20261006.md` + Artifact https://claude.ai/artifact/YGcZmCwxsxkr9127RLm8BH(HTML は md から変換スクリプトで生成・openapi から機械抽出)。EC2 未反映の変更(己・庚・辛・戊-b/c)は「EC2 反映は別承認」と注記 → 反映後に更新する。
