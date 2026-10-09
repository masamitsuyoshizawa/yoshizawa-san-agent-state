---
name: c-rehearsal-app-reads-canonical
description: C の演習で kg_api/app.py の文脈関数は kb_conn を通らず KG_INTERLOCKING_URI(既定=正本 9990)を読む・X3 は写しでなく正本を検査していた(2026-10-06 発見・同日是正 fefe3fe6)
metadata:
  node_type: memory
  type: project
  originSessionId: 7d23b295-de1f-476e-9436-0c42a7039f2a
  modified: 2026-10-06T02:47:17.999Z
---

2026-10-06、戊-a(keep-alive の写しの検証)で audit_c が写しで X3 不一致 11 を出したのを切り分けて判明。`kg_api/app.py` は C の接続に kb_conn を使わず `KG_INTERLOCKING_URI`(既定 `bolt://localhost:9990`=正本)と `KG_NEO4J_PW` を直接読む。`rehearse_env_c.py` も今回の測定も `KG_INTERLOCKING_URI` を写しへ向けていないので、**演習の中の X3(audit_ctx_generalization が app を import)は正本を読んでいた**(読むだけで正本は不変)。`KG_NEO4J_PW` に写しのパスワードを入れると正本への認証が失敗し、direction_lever_ctx 等 4 関数が発火せず不一致 11 になる(外すと 0)。

**Why:** rehearse_env_c の隔離の実測は「kb_conn が正本 URI を拒否するか」だけを見ていて、app.py の経路を見ていなかった。

**How to apply:** C の演習・写しで app の文脈関数を通すときは `KG_INTERLOCKING_URI` を写しへ向ける。是正(演習の環境で向ける+「app の C の口が写しを読む」実測を隔離の検査に足す)は ckb の道具の側で行う予定(rep 20261006-1144 §5)。app.py は共有なので変えない。

[[audit-run-in-main-tree]] [[claim-wider-than-implementation]] [[verify-what-the-target-actually-reads]]

**是正済み(2026-10-06・コミット fefe3fe6)**: `rehearse_env_c.rehearse_env()` が `KG_INTERLOCKING_URI` を写しへ向け、`app_reads_copy()` が app の C の口の db id を写しと照合(正本と同じ・区別不能・繋がらないは中止)。試験 test_audit_c 49・変異 4 通り・実物の正否両方を確認。dkb へ info 20261006-1157。
