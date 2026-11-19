---
name: mutant-check-compare-reasons
description: 変異の試し(mutants_*.py)を照らすときは、要約の数でなく行ごとの観測の理由まで比べ、例外の行を検出と分ける
metadata:
  node_type: memory
  type: project
  originSessionId: fdc4c948-da33-4847-b9a6-04212d37500d
  modified: 2026-10-27T11:33:32.047Z
---

coord が icc の直しを写しで照らすとき、壊した写し(変異)の結果は要約の数(n/n)だけで判断しない。行ごとの `mutant_observed` を前の版と比べ、`exception:` で始まる行(引数の数の違いなどの TypeError)は検出でなく「試験不成立」と数える。

**Why:** 2026-10-27、d5d9832 の照らしで要約だけを比べた。そのため、`budget_events` の 3 引数化に追随しない既存の変異 3 件が TypeError を detected=true と数えていたのを見落とし、rev が A3PC01(中)として挙げた。

**How to apply:** 照らしの手順で、全体の実行器の要約の照合に加えて、公開の結果の JSON の全ての変異の行を走査し、例外の行の数と、前の版から理由が変わった行を記録に書く。[[delegate-mechanical-checks-to-ai]]

**追記(2026-10-27 icc の 4e17a0d):** 観測の理由の欄が無い行は走査に映らない。adjreq1 では 171 行のうち 86 行(tools/mutants.py・i1・preeval・pilotfix)に欄が無く、その中に同じ種類の 7 件が隠れていた。今は共通の部品 `checks/mutant_verdict.py` が全ての行に `verdict`(detected/not_established/missed)と理由を残し、全体の実行器の要約 `mutant_verdicts` が例外を検出に数えた行・欄の無い行を数える。照らすときは「欄の無い行が 0 か」も見る。

**追記(2026-10-27 rev rep 1645・A3MR01・A3MR02):** 理由の差を 0 にするだけでは、基準の理由が最初から違う行を見逃す。送信の門に関わる変異は期待の理由を明示させる((b)+(a))。行を比べるときは、id の無い節(`send_prep_negatives` の 19 行など)を name で区別する。id で区別すると 1 行にまとまり、母数が 18 減る。

**追記(2026-10-27 icc・req 1723・rev A3MR01):** 送信の門に関わる対象の写し(一覧は `checks/mutant_targets.py`・78 件)には期待の理由を宣言した。照らすときは全体の実行器の要約 `mutant_targets` の matched が targets と同じか(一致なしの検出・宣言の欠けが 0 か)を見る。行ごとの差は `tools/results_rowdiff.py`(id の無い行は名で区別する。id だけで区別すると送信の準備の負の対照 19 行が 1 行にまとまる・A3MR02)。

**追記(2026-10-27 icc・req 1941・rev A3PK03・A3PK04):** 「評価の側」「送信の門を通らない」のような一律の除外の理由は、出口(pilot_cli の wrong_zero・停止札)への依存の経路で 1 行ずつ確かめる。間接の経路がある(由来の成立 → 差し戻しの停止の免除 → 公開の未確認 → 誤り 0。rev も coord も挙げていなかった)。`checks/contract_tests_pkscope.py` の FXPK-NEAR が判定の経路の関数を辿って未決の行を挙げる(通った枝だけ・辞書の差し替えは辿らない)。対象は 102 件。行の差の道具は expected・got と一覧の外の欄(`other_field_diffs`)も比べる。未決: 課題の一致・再生の一致の出口の条件は K6 に無い。

**追記(2026-10-27 icc・req 2122):** 新しい変異の実行器を作るときは、写しの表の全ての対照の ID を、run_rows に渡す「試しの関数の表」に入れ、対照をその表の関数から回す(FXMV-ALL-RUNNERS が照らす)。P・N の例のように別の照らしの道具の行を対照にするなら、ここで関数に包む。入れ忘れると全体の実行器が rc=5 になる(第 2 段の 1 回目)。対象は 151 件(第 2 段の門と出口の 46 件を足した)。

