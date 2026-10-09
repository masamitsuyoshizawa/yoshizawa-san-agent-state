---
name: v3-plan-b-stage1-complete
description: V3 案 B(B-KB の候補ごとに切り口を分ける)第 1 段の実装 B0〜B9-b が 2026-09-23 に完了。EC2/app.py 登録/段 8 は別承認で未着手
metadata: 
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-23T12:31:17.428Z
---

**2026-09-23、JR東海 B-KB の V3「案 B」第 1 段の実装(段 B0〜B9-b)が完了した**(fed が実装・coord が照合・rev が 3 回レビュー・bkb が受入)。

| 何 | 版(2026-09-23 時点) |
|---|---|
| `kg_api/sources/facet_backend.py` | `5cf3ebd3d158e88c`(1,673 行) |
| `kg_api/sources/facet_router.py` | `6885fc70564b6808`(290 行) |
| `kg_api/kb/scripts/test_facets_v3_freeze.py` | `564692c6221e3638`(2,816 行・**146 件**) |
| 設計書 `docs/設計_B-KB改善v3_API_20260921.md` | `1ca061ea41e4cc15` |
| 計画 `docs/計画_V3_案B_実装_fed_20260922.md` | `c9974b5d0e0c67a7`(改訂 14) |
| 測定 `docs/測定_V3_案B_B9a_fed_20260923.md` | `3401fb51f60de0b7` |

**契約**: 組の識別子は 4 つ組(切り口・`grouping`・設備の id・現象の正準名)。`options.detail`(`list`/`outline`/`full`)・`evidence_per_group`・`candidates`・`vocab_ref`。`meta` 15 鍵。**`contract_version` は新経路 `facets@v4`**(B9-b の切替で旧要求も新経路が既定。**backend の `search(multi=False)` の旧経路のコードは無変更で残っており、router の 1 行を戻せば旧に戻る**)。

**要求全体の上限 `GROUP_LIMIT` = 90**(利用者の決定・`dec 20260923-1936`)。**「全質問の Plan の最大以上」の「全質問」は実在の質問集合**(11 問・最大 54 組)。設備名を並べた合成質問(120 / 144 / …8,892 組)は**上限で止める対象**([[threshold-from-what-it-protects]])。

**EC2 へ反映済み**(2026-09-23 22:36・利用者が `dec 20260923-2145` で承認)。**コンテナ `jrtokai-v9-kg-api` の中**: backend `5cf3ebd3d158e88c`・router `6885fc70564b6808`・辞書 `dec71d2d71eb1b55`・**`/app/accident_kb_v7/config/equipment_aliases.json`**(**完成版はこの置き場を読む。無いと全組 `skipped`**)。**`app.py` `58fb9de6290ce35a` と nginx は触っていない。** 保全タグ `pre-planb-20260923` → `planb-20260923` = `latest`。

**段 8 の画面も EC2 に在る**(2026-09-23 22:43): コンテナ `jrtokai-v9-streamlit-facets-v3`・**`127.0.0.1:8513`**(**loopback のみ**)・イメージのタグ `facets-v3-20260923`。**URL では開けない**(**nginx の `location` 3 つは別承認**) — **見るのは `ssh -L 8513:127.0.0.1:8513 kb-demo-ec2`**。

**EC2 は fed からも読み取りで触れる**(`ssh kb-demo-ec2` → `docker exec`)。**書き込みは coord**(`OWNERS`)。

**第 2 段の候補**: 承認の一枚 項目 14 の「1 問 100 KiB」を実測が超える(`full` の 1 問の最大 **174.5 KiB**)。`list` の要求で `plan_preview` の重複が所要をほぼ 2 倍にする(3.1 + 2.9 ms・`dec 20260923-1913` の決19-2 で第 1 段は据え置き)。

**Why:** 版と「どこまでが承認済みか」を取り違えると、別承認の EC2 反映を勝手に進めたり、旧経路が残っていることを忘れて「戻せない」と報告したりする。

**How to apply:** 再開時は `docs/comms/INDEX.md` の fed 宛 open と上の版を突き合わせる。EC2・`app.py`・段 8 に触る前に利用者の承認を確かめる。

関連: [[v3-stage1-design-approved]]・[[v3-stage5-api-state]]・[[session-comms-protocol]]
