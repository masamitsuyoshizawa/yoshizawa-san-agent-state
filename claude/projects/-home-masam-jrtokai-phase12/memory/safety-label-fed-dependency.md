---
name: safety-label-fed-dependency
description: fed の探索木の描画が dkg_backend._SAFETY_LABEL を直接読む(決273)・名/鍵/語を変える前に fed へ req
metadata:
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-20T02:11:33.139Z
---

2026-10-20(決273・info 20261020-1110・fed 設計 改訂 11 §00 f5d879ddaf4cd0e9): fed の `dkg_tree.py` が停止の箱の文で `dkg_backend._SAFETY_LABEL`(A2/A3/A4 の安全区分の語)を実行時に import して読む。公開の名へは改名しない(受入済みの dkg_backend b5ff9d838581c4af を動かさないため)。

**Why:** 区分の語の正本を 1 つに保つ(Dify の本文の safety_mark.label と同じ語)。先頭 _ の名でも他セッションが依存している。
**How to apply:** dkb が `_SAFETY_LABEL` の名・鍵・語を変える前に fed へ req を出す。fed の試験が「名が在る・鍵 A2/A3/A4・語が空でない」を照らすので黙って変えると fed の試験が落ちる。A0・A1・null の語は無い(区分判定不能は fed の PREVIEW_BLOCK_LABEL)。関連 [[tree-redesign-candidates-history]] [[safety-mark-display]]
