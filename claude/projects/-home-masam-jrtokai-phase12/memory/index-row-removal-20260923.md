---
name: index-row-removal-20260923
description: A 系索引の本是正(個人情報を含む 7 チャンクの行除去)の方法・検証・見つかった副作用(refs_lookup の同点の並び・term_index の版ずれ)
metadata: 
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-15T12:24:02.577Z
---

2026-09-23 dec 0400: A 索引 3,913→3,906・refs_c_index 2,030→2,026・qaset 70→69(q000 除外)を**行除去**で是正。埋め込みは再計算しない。道具は fed の remove_chunk_rows / verify_chunk_removal / scan_removed_ids / probe_removal_effect。ローカル完了・EC2 は coord(rep 20260923-0700)。

- 是正前の全行から作り直すと faiss・npy・BM25 は現行とバイト一致(作り方の再現)。**term_index は meta より古い版から作られていて作り直すと 2,391 語の無関係な差**→除去分だけ外した(版ずれは info 0600 で起票)。
- bm25.pkl は中身が同じでも pickle のバイト列が実行で変わりうる(文字列オブジェクトの共有の差)。比較は中身で。
- **refs_lookup(kg_api/app.py)の np.argsort は安定でなく、同点の並びが配列位置で決まる**。4 行抜くだけで 208 問中 84 問の出力が変わった(実効 3 問)。ckb に渡した。
- 全走査はバイト単位で worktree 全部を見て「見た結果なかった」も記録する(coord の重視点)。id を持たない写し(rev の a_corpus)は本文 sha で照合する。

**Why:** 除去・是正は「他を変えない」ことが第一。作り直し方式は無関係な差を混ぜる。
**How to apply:** データの除去は行除去+前後の行ごと sha 照合+揺らぎを取ってから前後比較。順位の変化はまず同点処理を疑う。[[page-label-convention]] [[verify-zero-counts-before-reporting]]
