---
name: c-cooldown-race-breaks-on-off-pair-checks
description: C に届かない kg_api では、診断が 30 秒ごとに 1 回だけ C を試し直し、その 1 回だけ sources_down が unreachable になる → 旗の ON/OFF の対を比べる照らし(v3 の D2・D4)が巡り合わせで外れる
metadata:
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-20T11:13:40.636Z
---

`kg_api/kb/scripts/dkg_grounding.py` の C の接続は、失敗の後 `C_COOLDOWN_S = 30` 秒は試さずに CDeferred(sources_down の札「C:…:deferred」)、過ぎた最初の 1 回だけが最大 3 秒試して「C:記号一致:unreachable」「C:接地:skipped」になる(覚えはプロセスの中だけ)。C の接続先は KG_C_URI、無ければ localhost:9990 と網の別名 neo4j-cgraph。

2026-11-20 の EC2 手元演習の段 3 で、①〜③ の復旧の temp の v3 の判定が、この 1 回を含む撮りで外れた(D2_fired_on_minus_key_eq_off・D4_normative_on_eq_off が 1 件・違う欄は sources_down と uncertainty だけ・50 問とほかの照らしは全部通った)。34 回の撮りのうち 3 回。なぜその 3 回だけかは不明。EC2 から写した像の環境に KG_C_URI の鍵は無い(KG_INTERLOCKING_URI だけ)。rep 20261120-1436 で coord に EC2 の確かめと扱い(案 A = 対の違いが C の札だけなら撮り直す)を頼んだ。

**続き(2026-11-20 夜・c4・c5)**: C の索引(dkg_grounding._gazetteer の地名辞書・c_diag_view._cache の診断ビュー)は、kg_api の過程の中に 1 度だけ読み込まれ lru_cache で覚えられる(成功した時だけ)。読み込めた後は、文に駅・踏切の名が無い問いの診断は C に触れないので、C を止めても落ちた源に C の札は出ない(c4 で C の確かめが合格のままだった)。C の健康な時、応答に C を引いた印は無い(接地・c_insights とも C の有無に関わらず 0 件)。→ 「C の失敗の札 0」が言えるのは「この過程は C の索引を読み込めている」まで。今届くかは、容器の中の別の過程から同じ c_connect で繋いで見る(r351_c_reach_dkb.py 0217ec9c…046a・c5 で C を止めた時に CUnreachable で外れる事を確かめた)。

**Why:** 「同じ容器で旗だけ変えた 2 回は同じはず」という照らしは、時刻で変わる状態(試し直しの間隔)を持つ源が混ざると巡り合わせで外れる。前の版の回が通っていたのは偶然で、当日の EC2 でも C に届かなければ偽の外れで止まり得る。

**How to apply:** D-KG の診断や木を 2 回比べる照らし・台を作る時は、C に届くか(KG_C_URI と KG_C_PW・網の別名)を先に決め、届かないなら sources_down の C の札の違いを撮り直しか除外の対象にするかを、照らしの持ち主と決める。比べる前に、両側の sources_down の札を数える。関連 [[c-grounding-silent-failure]]・[[storage-order-changes-dkb-responses]]。
