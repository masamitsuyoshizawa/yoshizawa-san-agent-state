---
name: same-name-is-not-same-implementation
description: 同名関数の出現数を依存の数に数えた(bkb の独立 expand_equipment を fed 関数の呼び手にした)・依存は import を解決してから数える・「後段の検査は前段の後」と書くと終了条件と循環しうる
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-23T23:55:42.985Z
---

2026-09-24 の S3 実装計画(fed)で rev に 2 件指摘された(S3-6・S3-1)。

1. `git grep expand_equipment\(` の出現 11 本・36 か所を「fed の関数の呼び手」と書き、うち bkb の 6 本が壊れると主張した。実際は bkb の `knowledge_kb_v8/scripts/bkb/v3_expand.py` が同名の独立関数(4 引数・戻り `Expansion`)で、bkb の 5 本は `import v3_expand as X` でそれを呼んでいた。fed 関数の直接の呼び手は 5 ファイル 16 か所。
2. 「fed の実 DB 読取は S4 の後」と書いたが、S4 の終了条件(決34 の (3))が「fed の読み手と dkb の参照実装の 2 実装一致」だったので循環していた。是正 = S4 の中に「DB 準備完了 → fed の先行試験(API に組み込まない)→ 照合 → S4 終了」の段を切る。

**Why:** 同名の出現は依存ではない。`import` を解決しないと他所管への影響(=req が要るか)を誤る。「他セッションを壊す」という主張は自分の設計判断の根拠になるので、誤ると判断の理由まで誤る(加法案自体は正しかったが理由が誤り)。依存の順は「終了条件」まで含めて並べないと起点の無い環になる。

**How to apply:** 呼び手を数えるときは各ファイルの import 行を追って定義元を確かめ、「その綴りで 0 件」と「動的参照は未列挙」を分けて書く。段の依存を書くときは、相手の終了条件に自分の成果物が入っていないかを先に読む(入っていれば自分の段はその終了より前)。関連: [[claim-wider-than-implementation]] [[verify-filenames-before-citing]] [[assumed-current-state-without-checking]]


**実例 3(2026-09-27・coord)**: `llm_providers.py` はリポジトリに同名 6 本(中身 4 通り)。coord は直下の 1 本(9963129a)を読んで「Opus 5.5 未登録」と書いたが、kg_api が読むのは `kg_api/kb/scripts/llm_providers.py`(fa6c962b・app.py 22 行)。結論は偶然一致したが、参照するときは読み込み元を先に確かめる(OWNERS はパス付きで書く)。
