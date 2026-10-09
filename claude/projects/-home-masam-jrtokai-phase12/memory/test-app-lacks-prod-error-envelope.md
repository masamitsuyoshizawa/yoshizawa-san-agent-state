---
name: test-app-lacks-prod-error-envelope
description: 手元の台(R._app = ルータだけの TestClient)は本番 app.py の例外の包みを通さない — 422 は本番で {"error":{…}}・手元で {"detail":{…}}。合成の応答の形は EC2 の実物から取る
metadata:
  type: feedback
---

2026-10-09 表示改善 第 3 段の EC2 受入で判明: 本番の kg_api/app.py の `@app.exception_handler(HTTPException)` が detail を `{"error": {code, message, request_id}}` に包む。bkb の受入の台 `run_tree_s1_acceptance._app`(ルータだけ)はこれを通らず `{"detail": {...}}` を返す。プラグインの `_q_numbers_invalid` は detail しか読まず、EC2 では決212 の固定文が出ない。bkb の S6 (b)・S12 と fed の試験は手元の形で合成して合格にしていた(本番の形について何も言えていなかった)。

**Why:** 合成の応答を手元の台から取ると、本番だけにある層(例外の処理・ミドルウェア)の差が試験から消える。[[test-must-not-inject-what-prod-lacks]] の逆向き(本番にあって試験に無い)。
**How to apply:** 呼び手(プラグイン・画面)の試験に使う応答の形は、EC2 の実物(または app.py を通した TestClient)から採る。エラーの経路は特に。受入の限定に「手元の台は app.py の例外の処理を通さない」と書く。関連 [[claim-wider-than-implementation]] [[report-from-the-consumer-side]]
