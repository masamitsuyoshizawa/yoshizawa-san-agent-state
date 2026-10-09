---
name: dkb-audit-state
description: "B/D-KG 系監査の最新状態(2026-09-17: PASS 20/WARN 3/FAIL 0)と機械担保の所在・WARN 3 件の扱い"
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-13T09:08:55.130Z
---

B/D-KG 系(dkb 所有)の全項目監査は 2026-09-17 実施。是正後 **PASS 20 / WARN 3 / FAIL 0 / NA 0**(`knowledge_kb_v8/reports/audit_bdkg_20260917.md`・付録 `docs/監査付録_B-DKG系.md`)。

- 一括実行: `knowledge_kb_v8/scripts/dkg/audit_dkb.py --all --loo700`(X1/X1-2/X2/X3/X3r/X4/X5(b)(g)/X6/X6b/X7/X8/X9/X10)+ `scripts/x13_static_check.py [--profile c]`。
- X3r の回帰は `run_loo700_iso3.py --stamp <日付>_x3r --conds 1,2,3,4 --diagnose-only` → 基準 adb34caea6448d41 と 2,052 行比較(期待差分は ④ 6 件のみ=重複観測の 1 回化)。
- **WARN 3 は既知の継続件**: X6(死条件・常時発火)・X6b(意味照合マージン・dec 20260916-0500 で継続記録)・X8(基礎率支配 +23pt)。再提案せず記録を維持。
- 同一バッチ原則・513/566 の分離は `knowledge_kb_v8/scripts/dkg/eval_batch_guard.py` と台帳 `kg_api/kb/config/eval_sets.json`(集合の sha256 のみ)で機械的に停止する。テスト `test_eval_batch_guard.py` 17 件。
- 判定時コード(sx20260911)と現行コードで第 1 位が異なる問は 138/566。**公式値の再判定は絶対値を対外的に使う直前に 1 回だけ**(dec 20260917-0800)。
- **2026-09-15: 監査項目を 2 つ新設**。**X5(g-0) 静的資源の棚卸し**(定義=評価対象の事故から作られ診断で読まれる資源。従来の個別列挙が漏れの原因)と
  **X17 投入物が診断で読まれ効果があることの確認**(FAIL 2 / WARN 3)。詳細は [[ingest-effect-unmeasured]]。
- **2026-09-21: 判定器を L3 v2 へ版上げ**(パーサ・`judge_harness_sha`・max_tokens のみ。プロンプトは byte 同一)。基準値は `jv2_20260921` = c+p 294/566。詳細と「混ぜない」規律は [[loo700-baseline-judge-v2]]。
- LOCKS 運用(coord が明文化): 正本グラフ・EC2 に触れない静的 config/辞書 JSON の編集は予約不要。EC2 反映の段は coord が予約。

**Why:** 同じ監査を再実行するときの入口と、WARN を「新規の問題」と取り違えないため。
**How to apply:** 監査依頼が来たら上記コマンドを実行し台帳形式(PASS/WARN/FAIL/NA)で報告。WARN 3 は既知として扱う。関連: [[audit-procedure]] [[dkb-order1-3-status]] [[ingest-pipeline-enforcement]]
