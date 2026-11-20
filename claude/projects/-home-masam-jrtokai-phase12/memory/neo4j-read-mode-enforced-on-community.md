---
name: neo4j-read-mode-enforced-on-community
description: Neo4j Community の単一サーバーでも session(default_access_mode="READ") の書き込みはサーバーが Neo.ClientError.Statement.AccessMode で拒む(2026-10-28 実測・2026.03.1・ドライバ 6.1.0)。kg_api の読み取り専用はこの包みで担保できる
metadata:
  type: project
---

2026-10-28、一時コンテナ(neo4j:2026.03.1-community・password は token_hex)で実測: READ セッションの CREATE/MERGE/SET/DELETE/CALL db.createLabel はすべて `Neo.ClientError.Statement.AccessMode`(Writing in read access mode not allowed)。MATCH は通る。WRITE(既定)の CREATE は通る。

**Why:** Community 版は読み取り専用の利用者を作れない(dkb 確認)ので、kg_api の読み取り専用は「書き込みの Cypher がコードに無い」ことに依っていた(既定セッション 16 か所)。rev R331-8 が「READ が拒むかは未確認」と限定したため実測した。

**How to apply:** kg_api のプロセスでは kb_conn の `enforce_read_default_in_process()`(案 A・docs/設計_READ包み_kb_conn_coord_20261028.md)で既定 READ に。kb_conn.driver の既定そのものは変えない(投入側 275 ファイルが使い、ingest は書き込みセッション 8 か所)。受入は一時 DB で 3 生成経路(app.py:105・common.py:39・kb_conn:102)の CREATE が AccessMode になること。関連 [[next-phase-challenges-ab]] [[tmp-neo4j-password-leading-dash]]

**READ 包みの実装(決334・2026-10-29・coord)**: 枝 `coord-readwrap`(main から・受入前は main に入れない)。kb_conn に `enforce_read_default_in_process()`(`GraphDatabase.__dict__["driver"]` は classmethod → `staticmethod` で差し替え・`neo4j.GraphDatabase is GraphDatabase` なので import の形に依らず効く・包みより前に `from neo4j import GraphDatabase` したモジュールの呼び出しにも効く〔呼ぶ時点のクラス属性〕)。proxy `ReadDefaultDriver` は `session()` の既定に READ を補い WRITE 明示は KbConnError・`execute_query` の routing_ は "r" を補う・`with` は自身を返す。試験 = 固定 33 + 一時 DB(neo4j:2026.03.1-community・17687)実測 16 = 49/49・変異 4/4。**事実**: kg_api の直の `GraphDatabase.driver(` は 12 か所(設計 §1 の「3」は誤り・dkg_backend 8)・`execute_query` 0 件・neo4j 6.1.0 の Session は `_config.default_access_mode` で既定(WRITE)を読める。本物の app.py は最小環境(PYTHONHASHSEED=0・KG_ENABLE_REFS=0)で 0.6 秒で読め、enforce 失敗 → SystemExit(3)・ドライバ生成 0 を確認。
**Why:** 包みの効き方(クラス属性・呼ぶ時点)と kg_api の現物の数は、次に同じ包みを別プロセス(worker・Streamlit)へ広げるときの前提になる。
**How to apply:** 包みを足すときは `grep -rn "GraphDatabase.driver(\|\.session(\|execute_query("` で現物の数を先に数え、設計の数を訂正してから書く。受入の番号は R-1〜R-12 の 12 項目(R-8/R-9 は dkb 設計 §3-4 が足す。「無い番号」は dkb の grep 160 字切りの誤報を coord が現物で確かめずに写した・2026-10-29 訂正)。

**X1(2026-10-29・dkb 受入の発見)**: READ の既定(`default_access_mode`)は `Session.execute_write(fn)` に効かない(管理トランザクションは自分で WRITE を要求する)。`run()` と `begin_transaction()` には効く(サーバーが AccessMode で拒む・実測)。→ Session も proxy(`ReadDefaultSession`)にして execute_write を KbConnError に。試験 66/66(固定 38 + 実測 28)・kb_conn 551b1608bdf39a1a。**教訓**: 「既定を READ にした」だけでは要件 5 を満たさない。被覆表(設計 §10)に呼び方ごとに止め方と根拠を書く。

**受入合格・main 反映(2026-10-29 09:1x)**: dkb の受入 R-1〜R-12 合格(旗 OFF/ON で 38 操作 50 本 byte 同一・LLM 偽物・D3 digest 同一)。main a785e9c90(kb_conn 551b1608bdf39a1a・app.py 1ba2d19602035527)。残る限定 = R-9 実行時(worker 後)・X5(_run_transaction は静的 0 件)。**EC2 は未反映**(W2 の別 dec・compose の正本に KG_READ_ONLY_SESSIONS=1 を明示)。手元で kg_api を起動すると既定で READ 包みが入る(旗 0 で現状)。

追記(2026-10-31・決344): kb_conn の演習モードの正本拒否は、手元で公開している正本の口(9890・10090・9990・9790・10190・19892)なら host の書き方に依らず拒む形にした(main deed60b73・kb_conn.py f539fb5d22cf023d)。文字列の一致だけでは 0.0.0.0・[::1]・host.docker.internal・127.1・0x7f000001・localhost.・ip6-localhost・FQDN で正本に届いた。書き方の列挙は漏れる → 口の番号で照らす。CANONICAL を集合の一致で照らす試験(test_registry_write)は加法で落ちるので包含で照らす。


(索引から移した説明・2026-11-07)
- [Community でも READ セッションは書けない・READ 包み実装](neo4j-read-mode-enforced-on-community.md) — 決334 受入合格・main a785e9c90(2026-10-29)・EC2 未反映・直の driver 生成は 12 か所・X1 execute_write は Session proxy で止める・決344 で演習モードの正本拒否を口の番号で(書き方の列挙は漏れる)


(索引から移した説明・2026-11-20)
- [Community でも READ セッションは書けない・READ 包み実装](neo4j-read-mode-enforced-on-community.md) — 決334 合格(a785e9c90)・EC2 未反映・演習の正本拒否は口の番号で(決344)
