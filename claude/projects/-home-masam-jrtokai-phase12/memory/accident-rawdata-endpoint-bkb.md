---
name: accident-rawdata-endpoint-bkb
description: 決154 事故摘録の原本を返す API(/v1/accident/rawdata)の bkb 実装の状態
metadata:
  type: project
---

決154(2026-09-30・利用者判断・顧客確認済み): 受領した原本(PDF 348 件分・Excel 352 件)を EC2 に置き、API で acc_id から返す。B-KG は伏せた形式のまま。

- bkb 実装 = コミット 4d0a89fe: 新規 kg_api/sources/accident_rawdata.py(83b12cf0)・accident_router.py に口 3 つを加法(17a83f46・既存の関数は不変)・accident_backend は不変。試験 kg_api/kb/scripts/test_accident_rawdata.py 40/40(合成データ)。
- 契約: KG_ACCIDENT_RAW_DIR の manifest.json と files/ だけを読む・パスを受けない・files/ の外と sha 違いは 404(本文 1 種類)・環境変数なしは 503・一覧なし・meta は relpath を返さない。
- 2026-09-30 決155: manifest ef1b7261 は 699 件 701 ファイル(acc:0640 は後で目視確定)。ローカル照合 全部合格(meta 699・本体 701 一致・0640 は 404・既存の口は不変)= rep 1725・道具 check_rawdata_local.py。
- 2026-09-30 rev R154-1〜5 是正(85518f9c): 覚えをやめ照合したバイト列を返す(ストリームでない・最大 3.47 MB)・OS 例外を 404/503 に分類・ext 許容集合・files/ 基点の検査・--base-rev 必須(基準 4d0a89fe^)。試験 75/75・変異 4 種で落ちる。新 manifest 2478b411(700 件/702 ファイル)全一致・0640 一致 = rep 1742。
- 2026-09-30 rev 2 回目 R154-R1〜R3 是正(c27b5c42+ef2317b7): 型を先に検査+try・8 MiB 上限/大きさ照合/上限+1 読込・枠 4 を**送信の終わりまで**保持(weakref.finalize で捨てた応答も返す)・O_NONBLOCK。試験 102/102・変異 7 つ落ちる・700/702 一致 = rep 1811。
- 教訓: 2 段の守りは片方を外しても応答が同じで変異が落ちない → 段ごとに単独で見る試験を足す。
- 2026-09-30 18:31 EC2 反映済み(決158・rawdata-20260930・実装 58aa5074)。2026-10-01 rev 1818 申し送り 1 の試験 3 件を追加(badb06be・105/105・実装不変)。説明文の「捨てた時」は回収時の意・32 MiB は 1 プロセスの本文量 → 次にコードを変えるとき直す。

**Why:** 原本は個人名を含むが顧客判断で掲載可。git には入れない(accident_kb_v7/data/ の規則)。
**How to apply:** 照合でも原本の中身は読まず sha256 と件数だけを見る。関連 [[facets-instance-entry-acceptance-bkb]]。


(索引から移した説明・2026-11-07)
- [事故摘録の原本の API(bkb)](accident-rawdata-endpoint-bkb.md) / [EC2 反映(決154〜158)](accident-rawdata-api-item.md) — /v1/accident/rawdata・2026-09-30 EC2 反映済み

- **伏せた版の契約(2026-11-16・coord req 1939 への rep 20261116-0911)**: `/meta` の `masked` は上の階(包みなし)で項目の値を返す(f0ae34cc1・415d0ad7197d22ea から)。項目とファイルの真の食い違いが 1 つでも在ると対応表全体が不正 → 全部の事故の口が 503。EC2 の像 t328-20261028 は f0ae34cc1 より前(常に false の版 58aa50748877e66f のはず・推定)→ 伏せた版の配備で kg_api の反映が要る。router の /meta の docstring が「常に false」のまま(次の kg_api の便で直す)。PDF にそろえる案への推し = 値だけの xlsx を正のまま、見る用の PDF を bkb の道具で描いて足す(LibreOffice の変換は使わない)。
