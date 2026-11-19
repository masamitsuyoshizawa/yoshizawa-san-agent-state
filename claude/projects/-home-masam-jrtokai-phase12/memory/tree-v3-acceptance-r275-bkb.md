---
name: tree-v3-acceptance-r275-bkb
description: 決275(探索木の第 2 次修正 F1-2・旗 DKG_TREE_RENDER_V3)の bkb の受入 — 全項目合格(rep 1152・e9a0d67b)・画面の記録 rep 1219・決276/278 EC2 済み・決277 合格 rep 1730・決279 合格 rep 2014・決280 EC2 済み・決284 合格 rep 1133/1203・決286 合格 rep 1328
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-20T02:30:34.115Z
---

2026-10-20 coord req 1126(決273〜275・利用者承認)。受入設計 `docs/設計_受入_探索木_候補列挙と経緯_bkb_20261019.md` 改訂 1(102a0b4f9eb62ec7・コミット 4b5d59be)§9 に V3 を加法。

- 基準 B1 = ce2cb5a1(決272 の EC2 の版)の写し = 本体ツリー knowledge_kb_v8/data/eval/bkb/tree_r270/fed_w1c/ の pub_off 12d765f75629b72e(24 件)・pub_on 603cf17208d29c0a(34 件 = 通常 24 + t6 + 合成 9)。rep 2019 で先出し済みなので採り直さない。
- 項目: G1(OFF/OFF)・G1b(V2 OFF/V3 ON)・G2(V2 ON/V3 OFF・34 件同一)・G3(V3 ON の差は render の 8 欄 + meta.render_version だけ・版 dkg-tree-render/3)・G4 q→u 0 本と u への札「現状で<br>判断」・G5 u 先 s 最後(宣言順)・G6 前置きなし・G7 見える字 ≤240 と 13 行・段 3 つ・1 項目 1 行・G8 印・G9 停止の箱(list の停止行 + dkg_backend の _SAFETY_LABEL + ce2cb5a1 の PREVIEW_BLOCK_LABEL)・G10 文法 v3 の負例 5 種 + <BR>・G11 変異 6・G12 通常例の代替表示 0。
- 実データの停止は A3/intervention だけ(6 例 9 件)→ A2/A4/区分判定不能/欄の欠けは合成。
- fed へ 4 点を確認中(V2 OFF・V3 ON の振る舞い・空の段・区分の無い停止の行・v3 の関数名と build の呼び方)。改訂 12 の文で期待の文言を確定する。
- **受入 合格**(2026-10-20 11:52・rep 1152・req 1126 閉じ): W2 = fed-tree273 e9a0d67b(dkg_tree.py 0b2f789e521be121)・TREE_R270_TARGET=w2・段 prep/off/offv3/on/v3/tests/gram/check/mut。G1 24・G1b 24・G2 34 同一・G3 34(matrix_md も同一)・箱 281/281(段 ⓪〜④ を根と枝で全部)・停止の箱 45/45・凡例 45・通常例の代替 0・文法の負例 9×3・変異 6/6。期待 = 基準の版 ce2cb5a1 の _l1_parts/_lv_names(w1c_copy から読む)+ 設計 改訂 12 補 198e7c373bd59f0b の文。
- 落とし穴: 合成 box@N・sx_*@N は描画の間だけ上限を N に下げるので、期待も N で計算する(240 で計算して 31 件を偽の不一致にした)。e9a0d67b の中の設計書は補の前の版。
- 画面の受入の記録を読んだ(rep 1219・screens_v3.json 008002526f30c9f5): 欄は揃う・人の判読は coord 可(決275 追補 3)/ human の欄は null のまま → 正本を 1 つに決めることを推した。EC2 反映・旗 V3 ON は決276 で完了(t275on-20261020)。**bkb の決275 の手番は終わり**。
- **決277(第 3 次の修正)受入 合格**(2026-10-20 17:30・rep 1730): W3 = fed-tree277 f753db98(dkg_tree ca7b572d4b0d61be)・TREE_R270_TARGET=w3。決まった文は設計 改訂 13 §000(e6cc4cdf942a0e79)から spec277_from_design で読む。前の V3 = fed_w2/pub_v3(e346af728687d03d)と G3b。G13 左寄せ 55/55・D7 枝の停止 3 行 11/11(br_<code> の合成で 9 種 + 表外 1)・凡例 13 行・変異 12/12・試験 v3 33/0。受入設計 改訂 2 §9-8(67f9a8bbd239fd93)。
- 決278(第 3 次の EC2 反映)完了 2026-10-20(EC2 kg_api t277-20261020・coord)。人の判読の正本 = 決278 の追補(coord 可・利用者の目視待ち)。bkb の手番なし。
- **決279(V3 で図の下の L3 の表を外す)確かめ 合格**(2026-10-20 20:14・rep 2014): W4 = fed-tree279 c0cded3c(dkg_tree ffe1d0aad7af9014)・TREE_R270_TARGET=w4・設計 改訂 14 e779b98ab7d652d1。期待の表 = V2 の表から「| | 系統の候補(L3・上位 3 件) |」か OMIT_L3 より前を切り出す(v2_matrix_part)。G14 34/34・55/55・凡例 12 行・変異 2/2。前の V3 に表の代替の記録 0 件 → その照合は中身の無い合格と明記。受入設計 改訂 3 §9-9(b142f2ab1165c817)。決280 で EC2 反映済み(2026-10-20・EC2 kg_api t279-20261020・coord)。bkb の手番なし。
- **決284(決282 候補が外れる枝にも候補・決283 B 由来の原因名を図と表に)受入 合格**(2026-10-21): dkb e0e5d085(rep 1133・TREE_R270_DKB=w5d・views 無し B0 と同じ・戻すと決270 写しと同一・根 L1 名 = diagnose.candidates[].cause・dropped と levels の有無・変異 4)/ fed 5c6a59d5(= 975e537c・rep 1203・TREE_R270_TARGET=w5 = fed + dkb の 3 ファイルを重ねた写し・基準 = W4 + 回答つき 115 件(info 1115 で先出し・ans_*)・H1〜H6・注記の箱 19・不出(acc・[氏名]・manifest の stem 612)・変異 6)。落とし穴: 決270 の D6_no_b_name を決284 に当てない・名を合成に替えた例を名の照合から外す・manifest は accident_kb_v7/data/rawdata(名は出力しない)。観察: 公開 levels の L1 の key に事故 ID の形(決270 から・図には出ない)。EC2 は決285。
- **R284-1 是正 + 決286(見えない辺 qN ~~~ s)+ R284-3 受入 合格**(2026-10-21 13:28・rep 1328・W7 = fed-tree286 4efca327・TREE_R270_TARGET=w7・基準 = fed_w5)。不出の保証範囲 = 事故 ID の形・指定形式のマスク印・manifest stem(≥6)の 3 つだけ(未マスクの氏名一般は未検証・確認の版は不明)。名簿 manifest 2478b4119a46da39・items 700・stem 612 を記録に固定・空なら不合格・leak_selftest 5/5。parse_mermaid は ~~~ も辺に数える。受入設計 改訂 5(8b6a98f53df85f6c)。落とし穴(再発): 照合の辞書に整数を置くと pass の集計で落ちる → 別の変数に。

**Why:** 決270 と同じく、期待は上流(levels・list・dkb の定数・基準の版の定数)から作り、実装の定数は使わない。
**How to apply:** fed の実装の知らせが来たら、改訂 12 を読んで §9-3 の文言を確定 → W2 の固定の写し → G1〜G12。関連 [[tree-redesign-acceptance-r270-bkb]] [[collation-derive-expected-from-source]]
