---
name: kg-key-path-only
description: 外部用の KG API 鍵ファイルは値を読まず、パスだけを gpg 等の引数に渡す(利用者の指示)
metadata:
  node_type: memory
  type: feedback
  originSessionId: 40296ee3-0d44-4626-82d1-5c1ee51784dd
  modified: 2026-10-28T10:30:55.629Z
---

外部用の KG API 鍵は `~/.kb_external_key_20261028`(権限 600・識別子 cdec15eb = 末尾の改行を除いた値の sha256 先頭 8 桁)。分析セッションは値を読まず、表示もしない。ファイルのパスだけを gpg などの引数に渡す。識別子は利用者が示した値を使い、計算し直さない。許される操作は `stat` などの属性の確認まで。

**Why:** 2026-10-28 に利用者が明示した。露出の原因はシェルの履歴だった。値が会話の記録に残ることも避ける。

**How to apply:** 暗号化は `gpg --batch --encrypt --armor --trust-model always --recipient <受け手> --output <出力> ~/.kb_external_key_20261028` の形で行う。`--trust-model always` は、指紋を利用者が別の経路で照合した後にだけ使う。出力は暗号文で、確かめるのは `gpg --list-packets` の宛先の鍵 ID と `stat` だけ。手順の正本は本リポジトリの `docs/手順_外部用の鍵の受け渡し_20261028.md`。
