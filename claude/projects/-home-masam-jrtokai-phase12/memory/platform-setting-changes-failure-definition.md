---
name: platform-setting-changes-failure-definition
description: 基盤の「失敗時の再試行」を足すと失敗の定義そのものが変わりうる・挙動の判断の前に基盤のソースで条件を確かめる(Dify の retry で 2xx 以外が中断に化けた・2026-10-24)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-24T05:45:31.359Z
---

Dify 1.16.1 の http-request に retry_config を有効にすると、retry か error_strategy があるときだけ 2xx 以外が「節点の失敗」になり、再試行が尽きると error_strategy が無ければ実行全体が中断する(graphon/nodes/http_request/node.py 207〜219 行・coord が EC2 のソースで確認)。今の本番(どちらも無い)は 2xx 以外でも status_code を出して進み、後ろの G1 が SKIP する。fed は rep 20261024-1436 で「再試行は同じ GET を繰り返すだけで保証は変わらない」と、条件を確かめずに書いた(撤回・rep 1445)。

**Why:** 再試行は「失敗したらもう一度」ではなく、何を失敗と数えるかを変える設定だった。後ろの照らしが守っているから安全、という推論は、照らしまで実行が届く前提に依っていた。

**How to apply:** 基盤(Dify・Neo4j・nginx など)の設定を足す・明示する判断の前に、その設定が有効なときと無いときで経路がどう分かれるかを基盤のソース(または一時アプリでの予行)で確かめる。確かめられないときは「未確認」とし、場合分けの各側を「安全の性質は変わらない」と言い切らない。共有の置き場へ生成物を書いた後で保留になったら、本番の版に戻し、保留の版は別名で退避する([[ec2-dify-dsl-lineage]]・[[claim-wider-than-implementation]])。

結末(2026-10-24 15:13): 決309 追補 2 で retry_enabled: false を 3 節点に明示(生成の道具 9f079a03・D の DSL 355d3e91・main c4a3b8a6)。console は明示の false を保ち、null の欄だけ true に書き換える(coord の一時アプリでの陽性の対照つきの確認)。EC2 反映は別 dec。
