---
name: facets-instance-entry-acceptance-bkb
description: 決134〜138 facets の個体 entry 是正の bkb 分(受入設計 改訂 7・独立投影・受入判定器)の状態
metadata:
  type: project
---

facets の設備クラス軸に個体 entry(例 転てつ器164号)が混ざる件の是正(決134)。bkb は受入を担当。

- 受入設計 `docs/設計_受入_facets個体entry_bkb_20260928.md` 改訂 7(sha16 09f65f50bcc0dbc7・2026-09-29)。rev 3 回で確定(C1/C2 は coord が現物確認で閉じた)。
- 決136: 畳みは fed 改訂 3 §4-2 の 9 段・10 辺先まで採り 11 辺目で止める・畳み先 {class, unreviewed, 欠欄}・親なし個体はクラスの軸から外す・granularity_source は dkb の台帳だけ・門 T=216。
- 決138: 実装承認(ローカル)。受K3 の合否(|L_r|=0 合格・損失や説明できない利得は保留で利用者へ・未取得 1 件で全体保留)は承認済みで、**損失を見た後に替えない**。
- 実装済み(2026-09-29): 独立投影を主番号ごと(run_v3_bundle_digest ce980b31・v3_vocab_projection 5f5d1adc・コミット 208e6366)、dkb の合成写しと digest 全部一致(A 6320f015・B 5993d238・v3 読み直し 2e763fba)。受入判定器 facets_v2_acceptance.py(4d03f121・コミット 0d920128・自己試験 49/49)。
- 2026-09-29〜30: 実走の台 run_facets_v2_collect.py(97db0a05)で空回し(合成の札・sink)全合格。受K7 の除く欄に vocab_snapshot_id(fed 追補 1)。受入設計 改訂 9(0adcadae)。採用札の表 372 行と受K2 の 36 升が dkb と一致(返却版 dd713eac)。
- 2026-09-30 決143 の受入の測定(736 問・B 正本と語彙正本 v4.0・読み取りのみ): 受K3 = 判定保留(失った事故 4: Q-loo 2 = 親へ畳んで strict で外れ grouped に残る / Q-syn 2 = 親なし個体・説明できない利得 0)・他は全部合格。証跡 knowledge_kb_v8/data/eval/bkb/facets_v2/run_20260930-0204/(k3_holds d85a9da2)。rep 0210 で利用者判断へ。途中で bkb の読みの誤り 2 つ(注記の範囲は grouping かつ候補 id・候補は min_len=1)を直した → 判定器 2b0ce06a・受入設計 改訂 12(c4b58e2b)。
- 2026-09-30 決144: 利用者が保留 4 件を「意図どおりの変化」と判断し受入合格(受K3 の式は変えず個別判断として記録)。次は EC2 反映の手順書(fed・dkb)→ rev → 実行承認。EC2 後の受入(正本と戻した DB・d2)は別途相談。
- 2026-09-30 決145(親なし個体はクラスの軸に残す・決144 の合格は取消)→ 受入設計 改訂 14(3879cad1)・判定器 4231cd59(58/58・instance_only の出現を全組で不一致)。決146 の再測定(run_20260930-0331): 受K3 保留 = 失った事故 2(前回の保留①と同一・利用者は意図どおりと判断済み)・保留②は解消・他は全部合格・v1 は前回と全問同じ。rep 0336 で利用者判断へ。
- 2026-09-30 決147 受入合格(保留 2 は grouped の畳み先の組に在ると coord が確認)・決148 共通表 改訂 13 確定・決149 EC2 反映完了(kg_api 既定 v2・語彙 DB v4.0・プラグイン 0.3.10)。bkb の K2(EC2 の書き出しと独立投影 17,389 要素一致)は rep 0406。
- 2026-09-30 決152/153: EC2 反映後の受入 (a) 戻した B(正本の回と全問同じ・保留 2 は決147 と同じ扱いで合格)・(b) d2(0/736 で合格)。facets 個体 entry 是正の bkb 分はすべて完了。
- (旧)残り: 応答を集める実走の台(fed の v2 実装の後)・受入の測定は別の dec・札の確定(決137 の 80 行)と判定基準は未決。

**Why:** bkb は facet_backend.py を開かない(受入の独立)。期待は承認された文と辞書から独立に作る。
**How to apply:** 実走の前に Q-guide/Q-syn の digest を固定し、共有資源は LOCKS。関連 [[order-acceptance-search-followup-bkb]]・[[verify-filenames-before-citing]]。
