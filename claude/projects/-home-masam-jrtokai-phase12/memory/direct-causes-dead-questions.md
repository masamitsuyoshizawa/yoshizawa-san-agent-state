---
name: direct-causes-dead-questions
description: direct_causes(決297〜304)の状態: R1 で 336 本補う・手元の正本へ投入(決303)・EC2 反映も完了(決304・2026-10-24 01:57)・F1 は諮り中
metadata:
  type: project
---

D-KG 診断で「答えても候補が動かない質問」51.9% は全部、判別の辺(DISCRIMINATES)を持たない質問だった(dkb 実測・LOO 646・2026-10-23)。
- 案 A(決297)= 応答に旗 `direct_causes`(既定 false・OFF は byte 同一)で「観測から直接導かれる原因」の別欄。契約 = dkb 設計 `docs/設計_D-KG_direct_causes_dkb_20261023.md`(改訂 5 dde771ddcf175d20)・表示 = fed `docs/設計_探索木_direct_causes表示_fed_20261023.md`(改訂 9 eebf8c004c3d62a2)・受入 = bkb `docs/設計_受入_D-KG_判別の辺R1とdirect_causes_bkb_20261023.md`(改訂 5 43c68afcf0d2744c)。
- R1(決298/300)= 名指しの原因へ辺を補う 310 + 概念 26(ambiguous 2 は足さない)。積荷 `discriminates`・段 G0 + **G17**。候補・特異点・c+p は不変で、変わるのは次の質問と行動の欄。副作用 b_isolation 224 → 171 は受容。
- rev の指摘は計 7 + 5 + 2 + 2 で全部採り、dkb 是正版 main 2b8cfedc・fed 711caf68 とも bkb 合格 + rev 合格(2026-10-23 23:14)。
- **決303(2026-10-23 23:3x)**: 正本 D-KG(手元 10090)への投入を承認。承認ファイルの文字列 = dec「決303」・stem `discriminates__commit__<開始日>`・target_sha = edges.json の sha256。段取り = dkb commit 開始(G17 承認待ち)→ bkb が期待 digest を独立に固定 → coord が承認ファイル → dkb --resume → bkb 受入(件数 24,534 → 24,870・集合・digest・応答写し byte 同一・戻しは一時の台)→ LOO 基準の写し取り直し。fed-dc の main 取り込みも承認。**EC2 反映(D-KG 積荷 + kg_api 置換 + 0.3.19 + D の DSL)は手元合格後に別の dec。**
- 決302: V2 で B 実績由来の行の name/key を伏せるが、概念名が V2 で見えないことは保証しない(既存欄 12 か所に出る)。

- **決304(2026-10-24 00:42)**: EC2 反映 2 段。段 A = EC2 D-KG(neo4j-dkg・トンネル 19891 → 172.20.0.8:7687)へ同じ 336 本を --env ec2 で投入 完了(stem discriminates__commit__20261024_ec2・dec 決304・digest 15935392 → **e6fff51a820caeb4**・手元 27354365 と違うのは属性 dec/batch の値・受容)。bkb 前半合格・後半 = EC2 kg_api 応答の写し(inputs.jsonl base 1,345)を coord が EC2 上で再生。段 B(kg_api 5 ファイル t304・0.3.19 pkg 1c16ca87a395d4c0・D の DSL 44bac403)は材料を EC2 ~/t304deploy_20261024/ に配置済み・bkb 後半合格後に実行。注意 = EC2 kg_api の環境 KG_DKG_GRAPH_DIGEST=15935392 は固定値で実グラフと食い違う(docker cp では変わらない)。EC2 の 3 DB の認証は同じ値(user neo4j)。トンネルは ssh -f -N -L 19890:172.20.0.2:7687 / 19891:172.20.0.8:7687 / 19892:127.0.0.1:10190。

- **決305〜307(2026-10-24 完了)**: F1 = プラグイン 0.3.20(凡例の前の空行)・F2 = 答え済みの測定を次の確認から除く(dkg_backend 15d8aa84・dkg_tree_state 079e42a6・不明も除く・旗なし・受入基準 = 測定由来の 4 欄 + D6〔尽きた応答〕の派生 3 欄を除いて byte 同一)。EC2 反映 決307 = 1 便(kg_api t307・0.3.20・D の DSL 入れ直し)・bkb/fed 受入 全合格。残課題 = EC2 の遅さ(diagnose 0.58〜0.63 秒/回・Dify の道で枝の予算切れ 1)・C 系の接地なし 1・質問の側の asked のみ(dkb 諮り待ち)・同一追補の内部重複・R3。

**Why:** 投入後の digest は演習の台と一致しない(辺の属性 dec/batch の値が違う)。期待は件数・集合・応答の写し・先出し digest で照らす。
**How to apply:** 投入の文字列(dec・stem)は dec に書いたものをそのまま使う。EC2 の (0) で実際の digest を coord が測る。関連 [[t0-tree-stage1-ec2-deployed]]・[[r1-discriminates-acceptance-bkb]]。

**2026-10-24 00:06 決303 で手元の正本 D-KG へ投入済み**(stem discriminates__commit__20261024・run_id 41fbba7ffa8c8ef6・dec 決303)。digest 15935392c733b65c → 27354365273cb606(bkb が先に出した期待と一致)・関係 24,534 → 24,870・印の辺 336 = 一覧の集合。台帳 dkg 更新・dkg_ec2 は旧値のまま(EC2 は別 dec・coord)。写し r1_commit_20261024(MANIFEST dfbd55475ab240b8)= r1_baseline と 7/7 byte 同一・c+p 流用。commit の G0 は語彙 DB の接続の環境変数(KB_VOCAB_NEO4J_URI/USER/PASSWORD)も要る(台帳に vocab があるため)。

**2026-10-24 01:57 決304 完了(EC2 = kg_api t304・0.3.19・D の DSL 44bac403 旗 ON・D-KG e6fff51a)**。fed の受入 (a)(b)(c) 全部合格(rep 0132 保留 → 0152 → 0156)。
- 受入で学んだこと: 比べる基準は EC2 と同じ設定で手元を取り直す(KG_VOCAB_SOURCE=db・DKB_FLAGS_FILE で DKB_SINGULARITIES=1・DKB_RETURN_METHOD=1)= post_m3_ec2cfg2.json 8febf1a74483ec92。旗なしの post_m3 と比べると 22 問全部に差が出る。meta.flags は環境変数の旗しか載らない(設定ファイルの旗は載らない)。
- 1 回目の 22 問は bkb の 1,345 再生と重なり 25 枝が STOP_BUDGET。負荷なしで 0。EC2 は diagnose 1 回 0.628 秒(決287 は 0.576・手元 0.157)= 約 4 倍遅く、Dify の段 3 の要求は 5 秒の予算の際で 1 枝が止まることがある(手元 1.2〜1.5 秒)。coord の残課題。
- 問 19 の singularities の差は決287 の時点から在る EC2 と手元の差。
- F1(fed・決305・**main f49af309 に取り込み済み 2026-10-24 11:12**・0.3.20 dkg_diagnose.py 000bf38914c4f10e・木 fb85bf7140a1ce73・bkb 合格 0358・rev 合格 1052・EC2 は別 dec で F2 の dkg_backend と同便の見込み・既存の byte 同一の検査は undo_305 で決305 の 2 行を戻して照らす): プラグイン _tree_tail_lines が matrix_md の直後に空行なしで凡例を足し、凡例が表の行に描かれる(決296 から)。案 = 0.3.20 で凡例の前に空行 1 つ。22 問中 20 問で境目 1 か所・ほかの型 0。
- F2(dkb・req 0153): 答え済みの測定が次の項目・表の列に残る(_next_checks は asked の質問の本文でしか除かない・推定)。
- ページ本文の全体では B 事例の候補の「出典:」行に事故 ID が出る(もとからの設計)。R284-1 の検査は新しい節と木にだけ当てる。
- 【済み 2026-10-24 12:57・rep 1257】決306(main 5da6f5cd・dkg_backend 15d8aa84)の後: 注記を直し、22 問の基準を post306_m3_ec2cfg.json 81227a0d9f565118 に取り直した(前と差 0 = 22 問は答えなしで始まるので答えた測定が無い・陽性の対照で段 3 の Q4 が消えるのを確認)。EC2 の probe の段 3 の図は Q4 の枝が消えるのが正しい。もとの予定: test_dify_button_layer.py 271 行の注記を「決306 前の本物の口を真似る」に直す(keep_answered=True の試験 4 か所は層の守りとして残す)・公開の木 22 問の基準を取り直す(dkb 写しで木 11/74 が変わる)。
- 誤りの記録: bkb の dc_display_acceptance.py の pin を「main で止まる」と誤報(pin の当たるパスは固定の写し src_fed711)→ info 0358 で撤回。pin は値でなく当たるパスを見る。
- **決307(2026-10-24 13:01)EC2 反映 完了**: kg_api t307(dkg_backend 15d8aa84・dkg_tree_state 079e42a6)・プラグイン 0.3.20・D の DSL 44bac403(識別子 0.3.20)。fed の受入 合格(rep 1335): 22 問 bf96683f vs 81227a0d で 21/22(問 19 は既存差)・probe 3 通り failed 0・凡例が表の外に出る・段 3 の本文の「Q4 測定」3→1。本文をカードの見出し「■ 段 N の質問」で区切ると段ごとに数えられる。
