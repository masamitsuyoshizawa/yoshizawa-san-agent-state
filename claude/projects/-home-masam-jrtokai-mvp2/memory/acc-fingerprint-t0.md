---
name: acc-fingerprint-t0
description: acc の正本の指紋(T0・T1・T0add)の版と digest・取り方・新しい受け箱の権限の頼み(2026-11-12)
metadata:
  node_type: memory
  type: project
  originSessionId: b54d92f6-694f-48da-9c1b-bc1712afcb52
  modified: 2026-11-12T04:15:44.992Z
---

正本の指紋は `scripts/acc_fingerprint.py`(sha16 99077e6da4cd1b06・第 0 版のまま)で取り、git 外の `~/jrtokai-mvp2/data/acceptance/fingerprint/<label>_<時刻>.json`(600)に置く。

- T0(2026-10-29 02:08)・T1(2026-11-05 02:29): 根 raw 16・intake 16、content_digest 3caaf1c9bebd363d。
- T0add(2026-11-12 13:13・info 1314・acc 8fd1531): 追加の受領(目録_20261112)の後。根 raw 150・intake 16・intake_20261112 8、content_digest 535680f0d3e68881・full_digest d179d85758cb51bd。T1 → T0add は増えた 142 だけ。raw の 134 は目録の sha16 と種類に一致し、名の sha16 = 中身。
- 新しい受け箱 intake_20261112 の 8 ファイルは 644 だった。coord が 444 にした(フォルダは 500)。T0add-perm(2026-11-12 13:15・full_digest c3730790f2f4b9b1)と比べ、権限が変わった 8(644 → 444)だけで、ほかの差は 0 だった。content_digest は同じ。info 1314 の status に記録した(acc bcdfb8e)。**次の T1 は T0add-perm と比べる。**
- 取り方: `take --label <L> --root ~/jrtokai-mvp2/data/raw --root ~/jrtokai-mvp2/data/intake --root ~/jrtokai-mvp2/data/intake_20261112 --show-paths ~/jrtokai-mvp2/data/raw`。受け箱の相対パスは原本の名を含みうるので書かない。compare は 1 件ずつの行を出すので、画面には要約の行と根ごとの数だけを出す。

- T2reboot(2026-11-20 12:02・再起動の後の確かめ): 同じ 3 つの根で content_digest 535680f0d3e68881・full_digest c3730790f2f4b9b1(T0add-perm と同じ・変わりなし)。intake_20261117(ヒヤリングの回答の xlsx 1 つ・sha16 21bec3950bf663fb・444)は、指紋の根に入れていない(coord に、入れるかを尋ねた)。

- **第 1 版の道具と新しい基準(2026-11-20 12:35・info 20261120-1236・acc 0a0453b)**: `scripts/acc_fingerprint.py` の sha16 は 5180832fb5947193(第 1 版)。根ごとの digest・`:Zone.Identifier` を既定で外す(`--no-exclude` で第 0 版と同じ)・`compare --common-roots`・selftest 9/9。**T0add2**(これからの基準・4 つの根 raw・intake・intake_20261112・intake_20261117・印のファイルを外す): content_digest 3ad8f36182e7f11f・full_digest 22ddc3acf63a056d。T0add2-all(何も外さない・橋渡し): 2ac13b1ee48f6aa9・e8c413ea9a499b42。前の 3 つの根だけなら 535680f0d3e68881・c3730790f2f4b9b1 のまま(差 0)。印のファイルは intake_20261112 に 4・intake_20261117 に 1(消していない)。**次の観測点は T0add2 と比べる**。

**Why:** docs/01 §2 の 6(決2)で、読取に入る前に acc が T0 を取る。新しい原本を足した時も同じ。後の T1 は T0add と比べる。

**How to apply:** 次の T1 は T0add と比べる。予備検討の資料 3 つ(intake_20261112・afc18c4f2296fb5a・03e9dab7ed07240e・f9f473983c73e373)は、取り分けの案が acc に諮られるまで中身を見ない。図面と写真は、マスクと人の確かめまで開かない。

関係: [[acc-yardstick-phase0]]・[[acc-no-read-canonical-graph-before-doc-freeze]]
