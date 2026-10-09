---
name: federate-phase1-2-20260915
description: "federate 第 1 期(順 1〜4)・第 2 期(A〜D)完了(2026-09-15): 精度 72% は構造的上限・確定した改善は再ランクキャッシュ/到達記録/B 枠/2 段 API/較正表示・rev 指摘で案A「効果なし」撤回・TOLERANCE 正式採用"
metadata: 
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-12T04:30:48.277Z
---

federate の打ち手(2026-09-14〜15・dkb と同形式のメモ→fed+rev 案→統合→利用者承認):
- 第 1 期: 順 1 到達記録と評価整備(gold の B 証拠到達 24/50→c+p 96%・未到達 46%=支配要因は取得層の到達性)、順 2 再ランクの決定論キャッシュ(EC2 full 120→83 s・−30%・要求をまたぐ query 単位 LRU 256)、順 3 案D 印 ON 非劣化(70%=70%)・A ブロック配分は効果なし、順 4 案 3/案 8 は到達率 +1/+0 で記録のみ・B 枠 FED_B_KEEP_DIAG 既定 ON 非劣化(EC2 反映済)・候補表は既定オフ。
- 第 2 期: A 較正表示(synthesis.calibration=B 手掛かりの強さ・表示のみ・誤誘導低減は主張しない・閾値 0.15/0.40 版固定・独立確認群なし)、B 明示選択+2 段 API(/v1/federate/retrieve 1 s→/synthesize)、C demo_v10 根拠の構造欄+test_ui_smoke(fed 所管・EC2 コンテナ内で実行可)、D 未到達内訳(概念未登録 10・支持 0 が 10・双子のみ 4・窓外 2)。Dify 構造化欄は見送り。
- 精度は 72%(50 問・同一バッチ)で構造的上限(未到達 26/50 のうち 20 は語彙・データ側)。上積みの手は尽くした。
- rev(Codex)指摘で覆った結論: 案A 初見ルーティング「効果なし」→規範候補が A ブロック上限 8 件で LLM に 0 件到達=未検証へ撤回。案D 印は合成入力に入る(表示のみでない)→同一バッチで非劣化を確認。
- TOLERANCE(F 系採用条件): 非劣化=c+p 低下 ≤1・incorrect 増加 ≤1・決定論指標非悪化/改善主張=+2 かつ J2 方向一致。多数決・問数拡張・双子のみ層の再定義は「判定条件の変更」として別途。X6b は保留。
- EC2 最新: kg_api calib-20260915・streamlit-v10 calib-20260915。

**Why:** 第 1 期で「取得・提示の改良は正誤を動かさない」が同一バッチで確定し、軸足を誤誘導低減・時間・説明性に移した。
**How to apply:** federate の精度向上を再提案する場合は到達記録(fuse_input)と 4 値遷移表・TOLERANCE で評価し、案 3/4/8/A 配分は再掲しない。関連 [[federate-accuracy-improvement]] [[rev-session-codex]] [[fed-vs-dkb-comparison]]。
