---
name: tree-redesign-acceptance-r270-bkb
description: 決270(探索木の図の改善)の bkb の受入 — dkb 合格(rep 1910)・fed 合格(rep 1923)・240 字の是正も合格(rep 1930)・画面の記録 rep 2000・F1/F2 是正 合格 rep 2019・残りは人の判読の記録
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-19T09:34:19.484Z
---

2026-10-19 coord req 1812。受入の設計 `docs/設計_受入_探索木_候補列挙と経緯_bkb_20261019.md` 改訂 0(94c9eea32c15a37d・75d315a0・rep 1817)。正本 = fed 改訂 9 §0(8ef1e43481e68166)・dkb 改訂 6 §3〜§5(3dd2e854235c2d93)・決266 追補 13・rev 1803 §4。

- 項目: 旗 OFF の不変 A1〜A5(E2 1,636 = 4 群 base1345/safety0 111/trace1 60/obsex1 120・HTTP 1,345 = base 群だけ・内部状態 74・公開 22)・旗 ON の限定の差 B1〜B4・足した欄の独立の期待 C1〜C8(枝の L2/L3 は W0 の旧口 diagnose を「根の観測 + 仮の観測 origin=virtual」で回す)・図表凡例 D1〜D7・試験と変異 E1〜E3(変異 7)・通常例の省略の件数 F1(期待 0)。
- B0 採取済み(info 1834 で sha16 を先出し): 証跡 本体ツリー knowledge_kb_v8/data/eval/bkb/tree_r270/b0/(0700)。道具 knowledge_kb_v8/scripts/bkb/tree_r270_acceptance.py(b0 --resume / pub)・tree_r270_checks.py(除外・差のパス・self-test B4 13/13)。bkb コミット 093e801d。
- 台の落とし穴: base_env の KG_VOCAB_REGISTRY は作業ツリーから作られる → 本体ツリーのパスで上書き。例題 1 の手番 1 にも `q_numbers_json="{}"` を渡さないと番号の表が付かない。
- **dkb の分 合格**(2026-10-19 19:10・rep 1910・対象 dkb-tree270 5f41b0ef): 旗 OFF は写しの sha16 まで基準と同一・B1 74/74・C1〜C8 不一致 0・試験 32/32・変異 4/4。設計に無い場合 1 件 = 根が計算されないとき history = null(dkb に設計 §4-1 への追記を、fed に null を受けることを依頼)。道具 ce350ca4(tree_r270_runner.py の w0cap で W0 の _diagnose_core を横から包んで子の戻りを記録・rootstop は KG_DKG_URI のポートを替えて D-KG に届かない台)。
- **fed の分 合格**(2026-10-19 19:23・rep 1923・W1 = fed-tree270 cdf1052e = 4e7ed39e + main 55870c0c): A4 24/24・B2 24/24・D1〜D7 違反 0・F1 通常例の代替表示 0 件・変異 7/7(fed 3 = 畳み外し/240 超過/根を枠の外)。陽性の例は自作: 例題 1 で番号 1〜5 に答えた実データ(経緯 5 件・畳みあり)・本物の内部状態の名だけ 40 字に替えた合成(L1 を 3 件とも名あり+根拠外・L2 を 3 件に)・停止/候補0/根 null の合成。公開の木の答えの枝は children の type == impact だけ(3 つ目は unknown)・branch は {value,label,…}。
- 240 字の読み方 = 前置き(21 字)込みの節点全体(決270 追補 7)。是正 f1cc3159(dkg_tree.py c5da1aacb1733211)を再確認して合格(rep 1930・TREE_R270_TARGET=w1b・旧版で 248 字の合成が 215 字・通常 25 件の render は旧版と同一・v2 試験 38/0)。
- 画面の受入の記録を読んだ(rep 2000・fed rep 1958・screens.json 77c04aaed36d350f): 欄は揃う・欠け 2(人の判読の鍵が無い・meta.dkg_tree_sha16_now にパス)。F1 = 根の箱の代替の文が枝向け(「この答えの候補で現状とは別」)。**bkb の照合は代替の文の中身を見ておらず捉えなかった** → 是正の確かめで根の箱の文に「この答え」を含まない・枝の箱不変・旗 OFF 同一・通常例 render 同一を足す。
- **F1/F2 是正 合格**(rep 2019・a3dc38df・dkg_tree.py ce2cb5a1a9cc5459・TREE_R270_TARGET=w1c): 根の箱の代替の文 6/6(上限を build の間だけ下げる合成 box@N で段 ②〜④)・枝 30/30・印「(新)」190/190・凡例 34/34(設計 §0-4 は未改訂なので設計の文に追補 12 を当てて期待を作った)・是正前と比べ OFF 24/24・ON 25/25・変異 f1_back/f2_back 2/2。道具の誤り 3 つ(段 ① の「ほか N 件」を代替の文と数えた・段 ④ に印を探した・変異の段で変数 target をパスで上書き)は直した。
- 残り: 人の判読の記録(coord 追補 14 = main 投入後に 6 枚撮り直し → 決271 EC2 を利用者へ)。req 1812 は open。

**Why:** 期待を実装の出力から作らないため、基準を実装の前に採って sha16 を先に出した。
**How to apply:** 実装の rep が来たら、まずこの記憶と設計 §2 を読み直し、W1 の固定の写しを make_copy で作って sha16 を照らしてから回す。関連 [[tree-stage1-acceptance-bkb]] [[precommit-hash-for-independence]] [[collation-derive-expected-from-source]]
