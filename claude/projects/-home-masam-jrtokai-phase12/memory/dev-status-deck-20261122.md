---
name: dev-status-deck-20261122
description: 社内の現状説明の資料「開発の現状と、今の API との差」(2026-11-22・表紙 + 本体 7 枚)の置き場・作り方・中身の出どころ
metadata:
  node_type: memory
  type: reference
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-11-22T01:08:59.536Z
---

利用者の指示(2026-11-22 09:5x)で、11-18 の「事故摘録の開発の現状」(4 枚)を、名を変えて更新した。

- 置き場: `docs/資料_開発現状_今のAPIとの差_20261122/`(台本 `build_deck_20261122.py`・出力 `開発現状_今のAPIとの差_20261122.pptx`)。11-18 と 11-10 の資料は、そのまま残した。
- 作り方: `/home/masam/jrtokai-phase12/venv/bin/python3 build_deck_20261122.py`。部品は 11-10 の `build_deck.py`(ひな形 `docs/SMテンプレ.pptx`)。見え方は `soffice --headless --convert-to pdf` → `pdftoppm` の画像で確かめた(pptx のスキルの soffice.py は defusedxml が無くて動かない)。
- 中身の出どころ: 担当 5 つ(bkb・dkb・fed・tkb・ckb)に、同じ形の表(今の EC2 の形 / 手元の状態 / どこまで / 残り / 利用者から見て変わる事)で尋ねた rep(20261122-0959〜1002)。決357 追補 161 に記録。
- 枚: 1 全体の一覧(機能ごとの段の丸)/ 2 今、動いている API / 3 今の API との差の表 / 4 個人情報 / 5 追加と PDF / 6 正答率・暗黙知・確認の道具 / 7 ご判断と次の節目。
- 「今の API(EC2)」は、記録からの記述(EC2 は読んでいない)。不明のまま書いた欄 = 一括追加の窓口が EC2 で有効か・EC2 の容器の中の版が本体と同じか。

**How to apply:** 次に更新を頼まれたら、同じ形の問いを担当に出し、この台本を写して日付を変える。利用者の判断の答え(案 M・S・N・EC2 の日取り)が出たら、1 枚目と 7 枚目が古くなる。関連 [[route-questions-to-coord]]・[[report-from-the-consumer-side]]。
