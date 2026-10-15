---
name: mvp1-final-report-slides
description: MVP Phase1 最終報告(2026-10-16)吉澤担当分のスライド — 5 枚版(coord/pla/agy の 3 案・2026-10-10)とロング版 11 枚(2026-10-13)・元デッキのマスター・ビルダーの作法
metadata:
  type: project
---

- 置き場 `docs/MVP1_報告資料20261016/`。元デッキ(顧客資料・未追跡)「JR東海様_最終報告_261016 (吉澤部分抜粋).pptx」のマスター「14【SAT/SATAC】詳細ページ」= `prs.slides[6].slide_layout`。pptx/pdf は未追跡(顧客資料)・ビルダー .py と md は追跡。
- 5 枚版(2026-10-10): coord 案 `吉澤部分_スライド案_20261010.*`(build_yoshizawa_slides_20261010.py)・pla 案・agy 案。利用者の案の選択は未。
- ロング版(2026-10-13・利用者依頼「②③ を各 5 枚程度」): `吉澤部分_スライド案_long_20261013.{pptx,md,pdf}`・`build_yoshizawa_slides_long_20261013.py`(① は 5 枚版の節を流用・②-1 全体像/②-2 B/②-3 C/②-4 D/②-5 品質・評価/③-1 位置づけ/③-2 37 本の地図/③-3 決定論の口/③-4 LLM の口と横断/③-5 契約と運用)。出典 = 技術説明_基礎資料 1・3・API リファレンス 20261006。数値は 5 枚版と同じ正本値。
- 作法: 題名は 17pt で約 38 字以内(超えると 2 行になり lead と重なる)・小見出し 7.2pt は 4in 幅で約 40 字・表は row_table/chips/panel ヘルパ・soffice → pdftoppm で目視・md は pptx の文面から抽出(同じ高さの箱を 1 行に)。

- **最終版(2026-10-13・利用者指示)**: `吉澤部分_スライド案_final_20261013.{pptx,md,pdf}`・`build_yoshizawa_slides_final_20261013.py`(agy のビルダーを土台に 10 枚: p1〜4 agy・p5 C(agy 左 + coord p4)・p6 D(agy 右 + coord p5)・p7 agy p6 で D は「半数程度」+ 評価方法は LOO700 含め検討中・p8 agy p7 で知識源を SAT-DKE だけに札で図示・p9/p10 = agy p9/p10。agy p8・p11 は外した)。agy の「初見 110 問・未登録 167 問」の古い数値は外した。

- 提出用の補足資料(2026-10-13): `docs/提出_構造化データ抽出パイプライン仕様書_BCD_20261013.md`(第 1 版 e94faaa51b6166a7)。技術説明 基礎資料 3 §1〜9 を提出用に整形(5 原則・分業・段ゲート・B 16 段・C 工程と G0〜G10・D パイプライン・品質/評価/版管理・用語・付録)。D の評価は 51.9% と「評価方法は検討中」を併記。

- md → docx は `scripts/export/md_to_docx.py <in.md> <out.docx> [日本語フォント]`(python-docx・pandoc と docx-js は無い・見出し/表/箇条書き/太字/頁番号・2026-10-14)。仕様書の docx 版も同じ場所。

- 提出用 チューニング記録(2026-10-14): `docs/提出_チューニング記録_試行と精度改善_20261014.{md,docx}`(md bd0f8892ec883860)。記憶ファイル群(eval-improvement-progress・ckb-remediation-waves・fed/dkb 系)の実測値を時系列表に。撤回した試行も記載。

- **最終版 第 2 版(2026-10-14・利用者指示)**: `吉澤部分_スライド案_final_20261014.{pptx(a62feb111fa56ae7),md,pdf(未追跡)}`・build_yoshizawa_slides_final_20261014.py は旧 final を import して p7〜p10 だけ差し替え。③ を ② へ統合・p7 = C/D 同じ高さ(C 208 問の出し方と判定・D LOO700 の手順/正答＋部分の意味/内部指標の注意)・B は補足 1 行・p8〜p10 は技術用語を平易化、エンドポイントは「機能」・小字の名称は「機能名」(利用者指示 2026-10-14・「口」は不適切)。md は scripts/export/pptx_to_md.py で pptx から起こす。目視は soffice → pdftoppm(venv = /home/masam/jrtokai-phase12/venv)。

- p1〜p6 の平易化(2026-10-14): 新スクリプトの PLAIN 表(74 規則)で旧関数の出力を生成後に置換(p.text の改行は別 run になるので規則は行ごとに分ける)。最新 pptx sha16 235f2b4eabbcde6d。

- **最終版 第 3 版(2026-10-15・利用者指示)**: `吉澤部分_スライド案_final_20261015.{pptx,md,pdf(未追跡)}`・build_yoshizawa_slides_final_20261015.py は 20261014 版を import し p8/p9/p10 を差し替え・p2/p4 は SAT_PLAIN の置換 + 脚注。構成 = すべての知識源は SAT の中・標準機能(高度な RAG)と拡張機能 SAT-DKE・A は標準 SAT が主・B は両方・C/D/語彙は DKE のみ・技術詳細は概念に留める(ノウハウはストックマーク側)・抽出データは成果物として提出・p10 は SAT-DKE の応答時間でエージェントの生成 AI の時間は別。

- agy の見た目違い版(2026-10-15 13:29): `吉澤部分_スライド案_final_agy_20261015.pptx`(aa13dd6fcc95e252)・テンプレートと中身は 10/15 版と同一(文と数値を機械で照合・技術用語 0)・KPI ピラー/タイムライン/ヘッダ帯/ステータス色の番号列など配置だけ刷新。利用者が 10/15 版と選ぶ。

- **採用版(2026-10-15・利用者決定)**: `吉澤部分_スライド案_final_採用版_20261015.pptx` = 基本 coord 10/15 版・p8 だけ agy 版・最終頁は `JR東海様_最終報告_261016 (次フェーズ)統合_20261015.pptx`(①〜⑪ をインパクト順・想定インパクト列右端・単色・残論点 紺/対応方針 橙)。組み立ては scripts/slides/assemble_final_20261015.py(テンプレートから生成関数で作り直し、agy p8 と統合頁は同じレイアウトへ XML を写す・元 9 枚は drop_rel で関係ごと外す)。**生成済み pptx を再度 python-pptx で開いて保存すると孤児の slide 部品が重複名になる**ので、組み立ては必ずテンプレートから。

**How to apply:** 利用者のコメント反映は md のコメント → ビルダー修正 → 再生成。[[tech-talk-materials-20260928]]
