---
name: ckb-costdown-20260918
description: "C-KB の API コスト削減を実施(2026-09-18): G7 1 回 73.6 → 24 ドル(−67%)。回答側プロンプトの固定部を cachePoint 化。速度は改善しない(出力生成が支配要因)"
metadata:
  node_type: memory
  type: project
---

C-KB(図表KB)の回答生成の API コストを削減した(dec 20260918-1600・EC2 反映 costdown-20260918=latest)。

**構造**: 1 問の入力 52,659 トークンのうち **48,550(92%)が全問共通の固定部**(`SCHEMA_HINTS` 29,689 字 + `format_answer` の system 16,590 字)で、毎問フル再送していた。判定器は 2026-09-13 に cachePoint 済みだったが**回答側は未適用**だった。

**施策**(送信テキストは byte 同一を単体テストで固定): A0 計測の整備(`llm_invoke`・4 種 usage・`meta.llm_calls` の実カウント化)/A1 `format_answer` に `cache_system=True`/A2・A3 `cache_prefix`(**A3 は A2 とキャッシュを共有しない**=system が 199 字/479 字で別)/A4 `schema_summary` のキャッシュ(**interlocking 限定**+300 秒 TTL+`POST /v1/admin/schema_cache/clear`)/A5 リクエスト内再利用/A6 埋め込み再利用(G7 では効果ほぼ 0)/A8 ウォームアップ 1 問。

**結果**: G7 1 回 **73.6 → 24.02 ドル(−67%)**。生成だけなら −77%。キャッシュ読みが送信の 90%。品質は C 系の既存ゲート通過(correct 186→185・incorrect 0・施策起因 0)。**速度は改善しない**(1,110→1,179 秒・1 問 18.6→18.2 秒)。時間の支配要因は `format_answer` の**出力生成 13.4 秒**で、入力キャッシュでは縮まない。

**費用の過小報告**: 「生成 ≈7 ドル」は ckb の根拠のない外挿で誤り(出典は rep 20260913-2300 の「B2 実測の残り」だが B2 は再判定の測定)。実測は約 60〜67 ドルで、**G7 11 回で累計 約 590 ドルの過小報告**だった。dec 20260914-1030 に訂正注記済み。

**Why:** 判定器で実証済みの手法が回答側に未適用のまま、1 回 73 ドルの実行を 11 回続けていた。
**How to apply:** 費用の突合は `scripts/check_bedrock_cost.py`(**CloudWatch の `AWS/Bedrock` を ModelId で絞り G6→G7 の区間で切る**。Cost Explorer は日別かつ他プロジェクトと混在で使えない。EC2 のホストロールには `ce:*`/`cloudwatch:*` が無くローカルで `saml2aws login` 後に実行)。実測の教訓: `enrich_lookup` の発火率は **40%**(前提の 25% は誤り)、LLM 呼び出しは 1 回の G7 で **474 回**(従来の数え方 416 は 14% 過小)。速度を縮めるには出力側に手を入れるしかなく別件。関連 [[ckb-session-state]] [[rev-session-codex]]。
