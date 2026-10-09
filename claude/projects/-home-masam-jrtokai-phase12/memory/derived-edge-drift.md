---
name: derived-edge-drift
description: D-KG の支持辺 268 本は 08-25 生成のまま、08-27 の空原因修復(削除+新設)に追随せず 116 組が欠落。再生成・手順書変更・現行 API の質問変更は利用者判断待ち
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-15T08:30:07.278Z
---

**派生辺の追随漏れ(2026-09-16 dkb 特定・coord 受領)**: `SUPPORTS_IF_ABNORMAL` / `CONTRADICTS_IF_NORMAL` は `build_evidence_edges.py` の実行時点のグラフから派生する辺。最終生成は 2026-08-25(364 組)。08-27 の `fix_empty_causes.py --graph` が空原因 99 を DETACH DELETE し `cause_source` 付き原因 351+候補辺 534 を新設したが、支持辺は作り直していない。現グラフでは含意鎖の組 384 / 支持辺 268 / 欠落 116(全件 `cause_source` 付き原因)。364→268 の減少は推定。再構築手順書は支持辺(5)を意味照合拡張(6)より前に置き、空原因修復を載せていないため、手順どおりでも再発する。351 原因は NORMALIZES_TO 0 本なので案 2a の件数には効かないが、診断の支持・反証には効きうる(未測定)。

**同時に保留中の安全案件**: 現行 diagnose の next_questions に、取り外し・取替などを伴う質問がある(coord 集計 89/1266、うち kind=observe 35。例 `dkg:q:828f74ca`)。EC2 で経路が生きていることを確認済み。利用者が dec 20260916-0900 で案 A を採用。dkb が `safety_mark` 欄を加法で実装した(DKB_SAFETY_MARK・fed §10.1 の A4/A3/A2・語の候補・MS は method も見る)。647 事象で PASS。EC2 反映は req 20260917-0100 で承認待ち。Dify / federate の文字列表示には印が出ない。

**Why:** 加法の投入物でも、後段の置換で派生辺がずれる型。X17(投入物の効果を測らない)の隣に起票予定。
**How to apply:** 利用者の判断が出るまで、支持辺の再生成・手順書の変更・質問の生成側や dkg_equipment_class_map・kind 分類の変更をしない。派生辺を点検するときは、生成式を現グラフで再評価した集合と格納辺を要素で比べる(`ft_inventory.py --item 5` が出力する)。関連: [[ingest-effect-unmeasured]] [[graph-query-discipline]] [[fault-tree-stage3-fed]]
