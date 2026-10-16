---
name: deploy-dependency-in-rep
description: 反映の単位にするファイルが、未反映の別の変更(モジュール)に依存するなら rep に書く(決261 で dkg_backend だけを EC2 に入れると dkg_display が無く diagnose が ImportError・2026-10-16)
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-10-16T04:32:54.691Z
---

決260 の是正の rep 1320 では、`dkg_backend.py` 3bc16f95166c1077 の版と「EC2 反映は別承認」だけを書いた。この版は、決251 で足した `from . import dkg_display` を公開の `diagnose` の中に含む。決261 は「表示契約(決251〜256)は含めない」として、dkg_backend と eval_config の 2 本だけを EC2 に入れる手順を承認した。そのまま入れていれば、EC2 の /diagnose は呼び出しのたびに ImportError で落ちていた。木の経路(`_diagnose_core` を直に呼ぶ)は落ちないので、受入の写しの一部しか異常を示さない。反映の直前に dec を読んで気づき、手元で再現してから coord に止めるよう知らせた(info 20261016-1332)。

**Why:** 手元の main では依存先がそろっているので、試験も写しの照合もすべて通る。依存の欠けは、反映先にしか現れない。

**How to apply:** 反映されうるファイルを rep に挙げるときは、そのファイルが読み込む自分の他のモジュールのうち、反映先に未反映のもの(どの決で足したか)を併記する。依存の確かめ方は、反映先と同じ集合のファイルだけを一時の場所に写し、公開の口を 1 回呼ぶ。[[verify-what-the-target-actually-reads]] [[ec2-replace-deploy-config-in-files]]

追記(2026-10-16・決263-B の受入で発見): **他の所有者の守りの試験も、変更の依存先である。** 決251 で dkg_router.dkg_diagnose に intake を足したとき、fed 所有の `kg_api/kb/scripts/test_dkg_tree.py` が router の構文木を「承認された置き換えだけ」と照らしていたのに、その一覧に足さず、試験も回さなかった。10-15 から 1 項目が FAIL のまま残り、決263-B で既存の試験 21 本を回して見つけた(変更前の版でも同じく落ちることで、今回の退行でないと切り分けた)。**公開の口(router・schemas)を変えたら、その名を grep して、他の所有者の試験も含めて全部回す。**
