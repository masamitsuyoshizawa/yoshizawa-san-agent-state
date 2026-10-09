---
name: ckb-obligation-c-schema-info-to-dkb
description: ckb が C-KG のスキーマ(ラベル・鍵・関係)を変えるときは dkb へ info を出す義務(dkg_grounding/c_diag_view の所有を dkb にした条件・2026-10-05)
metadata:
  node_type: memory
  type: project
  originSessionId: 7d23b295-de1f-476e-9436-0c42a7039f2a
  modified: 2026-10-05T13:13:16.553Z
---

2026-10-05、`kg_api/kb/scripts/dkg_grounding.py` と `c_diag_view.py`(D-KB が C-KG を読む部品)の所有を dkb とした(ckb rep 20261005-2208 の推し・OWNERS 10 行目に coord が登録・決183 甲)。条件は 3 つ: (1) C-KG へ書き込まない (2) その Cypher が新しい C のラベル・鍵に依るときは dkb → ckb へ info (3) **ckb が C のスキーマを変えるときは ckb → dkb へ info**。

**Why:** 2 本の呼び手は dkb の dkg_backend.py と eval_grounding.py だけで、ckb の G0〜G10・audit_c は使わない。だから C のスキーマを変えても ckb の試験では D 側の破損に気づけない。

**How to apply:** C のラベル・プロパティ・関係を変える・消す・改名する作業(schema_keys_c.json が変わる作業)では、投入の前に dkb へ info を出す。2 本が読む主な鍵: Station・SignalPost・LOCKS_SWITCH・所属駅/station・crossing・SoundingRule。

[[ckb-session-state]] [[c-diag-view]] [[c-grounding-silent-failure]]
