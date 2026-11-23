---
name: rkb-stage1-design-state
description: rkb(推論 KB・決364)の段 1 の状態。利用者が承認の紙 1(送信なしの確かめ)を承認(決364 追補 37)・切る道具と入力の道具を実装(24577bd9b)・rev の確認待ち・実の束には未だ当てていない
metadata:
  node_type: memory
  type: project
  originSessionId: 0934ae6b-71b5-4dab-9416-391e81b1b2ba
  modified: 2026-11-23T16:59:18.793Z
---

rkb(推論 KB・決364・作業ツリー `~/jrtokai-phase12-rkb`・枝 rkb)は、2026-11-23 に起きた。**2026-11-24 01:4x に、利用者が、承認の紙 1(案 2・sha16 ade9f1d88d6755f8)を承認**(決364 追補 37・coord 経由)。範囲 = LLM に送らずに、手元の伏せた記録を、道具で数える事だけ。紙 2(LLM の呼び出し)・地域・D1・D8・D11・型 A の向きは、未承認。

今の位置(coord req 20261124-0142 の順):
1. 済み: dkb(群の一覧)と bkb(入力の束 A = 作る側と開発用 → rkb / 束 B = 最終 → dkb だけ)へ req(20261124-0144・コミット 4deb09452)。
2. 済み: 道具を実装 = **枝 rkb のコミット 24577bd9b**(`knowledge_kb_v8/scripts/reason/` の r1_common・r1_cut・r1_inputs・r1_ncoarse と config 2 つ・試験 68 件合格・変異 9 つを捕まえた)。**rev の確認まで、変えない。** rep 20261124-0156。
3. これから: rev が通したら、**実の束に当てる前に、coord に一言**(当てる版・読む置き場・出す数)。
4. 作る側と開発用で、型と語の表を直す(最終の群は触らない)。
5. 道具の版を固定 → dkb が最終の群を 1 回だけ回す(`r1_ncoarse.py`)→ 3 つの数。**数が出たら、判じずに、そのまま coord に知らせる。**

文書の版: 設計の表 改訂 3 = `docs/設計_決364_推論KB_段1_rkb_20261123.md`(sha16 1b99de28a03be9c5・コミット 24577bd9b・§17 > §16 > §15 > §1〜§14)。試験の表 改訂 3(sha16 d3b106175de7dcb0・同)。承認の紙 2 案 2(sha16 853e756fd9197459・コミット 4c694e6fa)。上限の数えの表 改訂 2(sha16 13425aaebb3c96a6・コミット a24c1c7d9)。

決まった形: 門に使う数は、場面 イ の、入力を作り得る最終の代表の問いの数(切る型に依らない)だけ。8 以上 = 続ける / 5〜7・1〜4 = 「進む」に届かない → LLM を呼ばずに元の試しを終える / 0 = 場面 イ は比べられない。条件を、数を見て動かさない・数え直さない。元の試し = 計画のまま。広げる案は別の試し。

**Why:** rev と coord の向き = 評価の条件を緩めない・最終の群を 1 度だけ見る・rkb は最終の群の行と正解を見ない。**自分の誤り(11-24)**: 「実質同一の当たりは、作りの上で、ほぼ出ない」を推定として強く書いた → rev の指摘で、未測定の仮説に訂正(rep 20261124-0103)。

**How to apply:** 再開したら、先に `INBOX.md` と `python3 scripts/comms_unread.py`。試験 = `/home/masam/jrtokai-phase12/venv/bin/python3 -m pytest knowledge_kb_v8/scripts/reason/tests -q -p no:cacheprovider`。道具の出力の先は `accident_kb_v7/data/reason/` の下だけ(まだ作っていない)。紙 2 の承認まで LLM を呼ばない。紙 2 の前に残る直し = 型 A の向き(主の構成は人が確かめた段だけにする案 = 利用者に諮る点)・紙 2 の §1-3 の日またぎの字。待ち = rev・dkb・bkb(req 20261123-2318・2351・20261124-0144)。関連 [[why-reasoning-kb-r364]]・[[route-questions-to-coord]]・[[estimates-not-labeled-as-measurements]]。
