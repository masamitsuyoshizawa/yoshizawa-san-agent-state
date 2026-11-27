---
name: r367-single-entry-v2-endpoints-fed
description: 決367 追補 2(2026-11-28)の fed の分 = 1 件ずつの投入の画面・v2 の口の一覧(暗黙知 T・推論 R)・正式の事故 ID ACCR の響き。設計の案 改訂 1 と諮る点
metadata:
  type: project
---

2026-11-28 00:51 設計の案 `docs/設計案_決367_1件ずつの投入の画面とv2の口の一覧_fed_20261128.md`(改訂 0・sha16 2516b990fb2106a6・本体 e8ea280b4)。rep coord 0051(needs_user_approval: yes)・req bkb 0051(B1〜B6)・req tkb 0051(暗黙知の口で ACCR も受ける)。

- 1 件ずつの投入 = 今の /v1/intake/*(1 件の job)の上に作る・状態の機械は変えない・足すのは timing・visibility・名簿の役(投入者/承認者)と、console に 2 ページ(投入・反映の前の確認)。反映までの時間は機械と人の待ちを分けて測る。受入の試験(仕様書 6 ③)は人が書いた問いで。
- 諮る点 U1 反映の前の人の承認(推し 必須)・U2 承認者は別の人・U3 PDF と Excel・U4 投入者には受付と進み具合だけ。
- v2 の口 = 道は /v1 のまま加法。T /v1/tacit/accidents/{id}・/v1/tacit/lookup、R /v1/reason/candidates・next-check(仮)、/v1/accident/ids/{id}、統合の口の層の出所の印。MCP は upstream_client.py の許す一覧で守る(/v1/intake・/v1/eval は禁じたまま)。MCP の新しい道具 4 つ = プロンプトの変更 = 人の承認。
- ACCR: 4 か所(MCP normalizers・UNI router・accident_rawdata・dkg_tree)とも ACCR を通さない(当てて確定)。**dkg_tree.py の _ACC_RE は事故 ID を木の字に出さない漏れの門 = ACCR を止められない → 新しい ID が B に入る前に fed が直す**。
- 付記: MCP に 7a6e245(formatters)が入った。見張りの合格は 9642bfc の版 = 次の回で当て直す。

**Why:** 利用者の決め(決367 追補 2)。仕様書 4.(1) ②・6 ③・完了基準 4 が今の計画で一番手薄(coord)。
**How to apply:** bkb の B1〜B6 と tkb の返事、利用者の U1〜U4 を待ってから改訂 1。実装は承認と rev の後。関連 [[r365-test-env-fed]] [[r366-two-metrics-fed]] [[bulk-intake-window-design-fed]]
- 00:53 tkb は 2 点とも合意(rep 0052): 暗黙知の口と道具(write.py・approve.py)で acc:NNNN と ACCR の両方を受ける・JR の暫定 ID は会話の欄 jr_accident_id に保管だけ・時期は (d) の後。tkb の問い(両方の ID を持つ事故)への fed の推し(info 0053)= 片方で引いても両方の会話を返す・暗黙知の口は表を持たず kg_api の ID の別名の解決(/v1/accident/ids と同じ部品・表は bkb)で組に広げる。bkb の B4 の答えが来たら tkb へ知らせ、設計の案 改訂 1 に入れる。
- 00:56 改訂 1(sha16 d7d35b0e00cd7df6・本体 d75737fe2)= bkb rep 0053(B1〜B6・bkb 設計の案 改訂 1 267fbce7724d34ee §3-5)と tkb を入れた。**反映の前の確認 = G2(伏せ)と G3_SPAN(要点・抽出の後)の 2 回の待ちを 1 つの流れに見せる**(要点は G2 の時点に無い)。**移行の間は新しい事故も acc:NNNN(0701 から)+ ACCR の欄 = 全部が 2 つの ID の組**。1 件の機械の時間 = 偽の LLM で 回 1〜3 各約 30 秒・適用 約 100 秒(bkb 実測・合成)。ACCR の日付 = bkb は発生の日を推す(JR の暫定 ID の頭 8 桁と 52/52 同じ)・ID 不変の決まりで fed も異論なし・利用者の決め。次 = 利用者の U1〜U4 と日付の決め待ち。
