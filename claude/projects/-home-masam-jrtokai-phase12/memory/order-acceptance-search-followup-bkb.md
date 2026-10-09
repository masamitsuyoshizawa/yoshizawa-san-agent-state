---
name: order-acceptance-search-followup-bkb
description: 決102 の bkb 分 — search と followup の順の明示の受入の設計(2026-09-26)と、followup の次の質問がハッシュの種で変わる発見
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-09-26T08:16:09.840Z
---

決102(2026-09-26 17:11・rev の要是正を受けて計画を改訂)の bkb 分。設計書 `docs/設計_B照会の順_受入_search_followup_bkb_20260926.md`(改訂 1・7b3224390db48d67・110 行・コミット a228931b)を dkb の計画改訂へ合流(rep 20260926-1715)。**実装はしない**(rev の再確認の後に別承認)。

- followup(`accident_backend.followup` → `accident_diag_engine`)の順が決まらない所 F1〜F6: 上位 80 事故の同点(sorted は安定・照会に ORDER BY 無し)・原因の得点の加算の順(浮動小数点)・原因の同点・上位 4 の境目・**F5 次の質問 = `set` を回す順 = PYTHONHASHSEED(保存の順とは別。同じ B でもプロセスごとに変わりうる)**・説明文。F1〜F3 は DE.diagnose(D-KB が使う)にも同型。
- 鍵の向きの案: 事故 id DESC(facets・fed 案 1/3 と同じ)・文字列 ASC(repair ASC と同じ)・得点は生の値で DESC(表示は丸めたまま)。
- 受入: search AS1〜AS6(合成は使い捨ての Neo4j に 2 挿入順)・followup AF1〜AF6(偽の client で DB 無し)。合成の陽性対照で旧版に差が出ること(0 件は合格でない)・実データは O1 と同じ一対(正本 ↔ 書き出しから戻した B)。LLM 0。

**Why:** rev ORD-1(followup の漏れ)・ORD-4(search・followup の受入が無い)への答え。EC2 とローカルの一対は保存の順が効かないと分かった(dkb rep 2303)ので、実データの一致だけでは根拠にならない。
**How to apply:** dkb の改訂版と rev の再確認を待つ。実装の承認が出たら、`accident_backend.py`(dkb と共有)・`accident_diag_engine.py`(dkb・bkb・fed 共有)は合意の上で直し、設計書の AS/AF で受入を回す。関連 [[dkb-coverage-singularity-bkb]]・[[storage-order-dependent-responses]]。

**2026-09-26 17:18 coord の判断(info 1718)**: F5(ハッシュの種)は本件に含める・dkb の一覧に「保存の順 / ハッシュの種 / 加算の順」の 3 列・鍵の向きは bkb 案で統一(事故 id DESC・文字列 ASC・生の得点 DESC・表示は丸めたまま)・DE の補助関数の共用は可(dkb と合意・実装は承認の後)・既存 X3r の PYTHONHASHSEED 検査は維持。bkb の手番は dkb の改訂 → rev 再確認 → 実装承認の後。

**2026-09-26 17:28 改訂 2(ef04791a・rep 1728)**: 改訂 1 は API が読み込まない `accident_kb_v7/demo/accident_diag_engine.py`(b780c8b3)を読んでいた。**API が読むのは `kg_api/kb/scripts/accident_diag_engine.py`(82402eb9・config の V7_DEMO = V8_SCRIPTS)**。followup が使う関数は中身が同じで所見は不変・diagnose は API 版で既に鍵あり。coord の規則(info 1726): 合意済みの鍵は変えない(diagnose の事故 id 昇順・原因名 昇順)・新たに足す所は id DESC・文字列 ASC・生の得点 DESC → followup は diagnose と同じ補助関数で昇順・search は a.id DESC。**教訓: 同名のファイルが 2 か所あるときは、import の経路(config)で読み込まれる方を先に確かめる**([[same-name-is-not-same-implementation]]・[[verify-what-the-target-actually-reads]])。

**2026-09-26 18:25 改訂 3(83080e23・rep 1825)**: rev の低 1 件(決103)。F5 の「API の再起動のたびに変わりうる」は誤り — `kg_api/app.py` 18〜19 行が PYTHONHASHSEED=0 を起動の条件(fail-fast)・EC2 compose も 0 → 本番では揺れない。依存(種を替えた独立プロセス)は在る。**教訓: 言語の一般の性質(set の順はハッシュの種)から本番の振る舞いを書く前に、起動の条件・設定を確かめる**。

**2026-09-26 18:49 rev 3 回目(rep 1849)で改訂 3(83080e23)は残件なし = 確定版**。残りは fed の fed_eval の費用管理 4 件(決104)。実装の承認は fed の是正と rev 4 回目の後。bkb の手番は実装承認まで無い。

**2026-09-26 21:04〜21:30 決107 実装と受入(bkb 分)完了**: accident_backend.py 39c75c96 → 576820f1(01dc8796・dkb と合意・エンジン c9e8979d は dkb の単独コミット c068a2b1)。合成: AS1/AF1 11/11・AF2・AS2 合格・陽性対照 AF3(4 例)・AS3((a)(c))。段 3(正本 対 戻した B neo4j-order-b 18692): AS4・AF4 全問一致(新版の出力 3 ファイルは sha 336663aa で同一)・陽性対照 AS5 1 問・AF5 10 / 種 311 問。AS6: search の旧 対 新 は k6 87・k50 654 問(顔ぶれは 4・3 問だけ・残りは並び)。AF6: followup 404/646 が変わる。台 = test_order_followup_af.py・test_order_search_as.py・test_order_static_as1_af1.py・run_order_stage3_bkb.py。rep 2115・2130。次 = coord が fed の評価と合わせて EC2 反映(O4)を諮る。

**2026-09-27 決112 完了(bkb 分)**: O4 = EC2 の API の search 666×2・followup 646 がローカルと一致(rep 1653)。(3) X3r-bkb = `audit_x3r_restored_bkb.py`(99c785dc)の `check(restored=None)`・dkb が audit_dkb の旗 `--x3r-restored`(--all に含めない・約 40 分)から戻した B を渡して呼ぶ。単独・audit_dkb の 2 回とも PASS・陰性対照 旧版 db532a9c は 1・1・10 で FAIL(rep 1801)。D-KB は D-KG の保存の順で 151/646 FAIL(dkb の所管)。
