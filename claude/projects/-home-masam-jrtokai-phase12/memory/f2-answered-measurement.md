---
name: f2-answered-measurement
description: 決306 F2 答え済みの測定を除く — 受入完了(追補 10)・main 5da6f5cd に取り込み済み(dkg_backend 15d8aa84・dkg_tree_state 079e42a6)・EC2 は 0.3.20 と同じ便で別 dec(2026-10-24)
metadata:
  type: project
---

決306(2026-10-24 利用者承認): `_next_checks` の測定の側に答え済みを除く処理が無かった(質問は asked で除く)。結果の値に依らず除く・旗なし。

- 実装はブランチ dkb-f2(e0a8e572 実装・1f4cce76 試験/道具/設計 §7)・dkg_backend 1c51540c4952c716・未 merge。
- 答え済みの集合は `_contract_filter` の**前**の観測から作る(復旧後の正常も答え済み)。
- **actions・actions_investigation(・ask_next)も変わる**: measure の行は next_measurements から 1 対 1。rep 0200 の「next_measurements と木だけ」は誤り → 受入基準の追補を coord へ諮り中(rep 20261024-0406)。
- 再測(f2_20261024・MANIFEST 245cdf97dd0d13b4): E2 base 223/1,345・obsex1 19/120・HTTP 223・木 11/74・LOO 646 byte 同一。判定道具 `compare_answered_measurements.py`(D1〜D5)。
- 追補が決まったら bkb 受入 req・rev 差分確認 req を dkb から出す。EC2 は別 dec。質問の側(asked のみ・E2 242 件)は別に諮る。

**Why:** 派生欄を見落として「ほかの欄は byte 同一」を dec に書かせた。
**How to apply:** 欄を除く直しの範囲を見積るときは、その欄から作られる欄(actions など)を grep で全部たどってから書く。関連 [[absence-claim-read-whole-path]]・[[direct-causes-dead-questions]]。

**2026-10-24 追補 1・2**: 追補 1 で 4 欄の基準を承認。bkb 0434 合格の後、rev 1052 が要是正 4: R306-1 木は normalize_observation で不明の測定を落とし answered に入らない(→ 正規化前の入力から集合を作り answered_extra で根と子へ)・R306-2 尽きた境界で convergence/self_assessment/confidence も変わる(既存写しは 0 件・推し b = 境界 D6 に限り許す)・R306-3 比べの道具が old==new を skip・R306-4 ID だけの判定。是正 f0816183・設計 d2e033fb(改訂 7)・道具 改訂 2 32d38a4511c5ab0d・写し f2b = f2 と 7/7 同一。教訓: 入口が 2 つ(診断の口と木の口)あるときは、両方の正規化を通る値で集合を作る。比べの道具は「同じなら合格」で飛ばさない。

**2026-10-24 12:54 main へ取り込み**: rev 1249 合格(道具 改訂 3)・bkb 1222 合格 → 決306 追補 10 で受入完了。dkb-f2 を merge(a50c663a)→ main 5da6f5cd。基準の写しは f2b_20261024(MANIFEST 245cdf97dd0d13b4)。EC2 は dkg_backend と dkg_tree_state を同じ便で(片方だけだと木の口が落ちる)。
