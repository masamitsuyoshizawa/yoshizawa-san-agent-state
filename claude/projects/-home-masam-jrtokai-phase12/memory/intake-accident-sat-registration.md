---
name: intake-accident-sat-registration
description: 決349(2026-11-04)事故摘録の追加を本 PJ の KB と SAT の両方へ登録する画面の検討・登録先は 3 か所・段 0 の確認待ち
metadata:
  type: project
---

決349(2026-11-04): 追加データの検討を再開。先に事故摘録の追加を、デモとは別の画面から、本 PJ の KB(B・D-KG)と SAT へ登録する(最終案ではない)。検討 `docs/検討_追加データ_事故摘録をSATと本PJへ登録する画面_coord_20261104.md` 改訂 1。

- 決定: SAT へはマスク済みの版だけ(原本は送らない)・入力は PDF と Excel の両方(工程に Excel の読み取りを足す)・試作は手元(W1)で既存の薄い登録画面(kg_api/intake_console)を広げる・SAT の段は検証用の置き場ができるまで送る直前で止める。
- **登録先は 3 か所**: 本 PJ の KB・SAT・**エージェントの手元の索引**(backend/app/incidents・Excel から 484 件。名前が一致しない SAT の結果をエージェントは捨てる)。
- SAT の事実: 追加 = `POST /upload_document`(file・index_name・vector_store・dataset_id)・削除 = `delete_document`(file_name)・**認証なし・agent EC2 の 127.0.0.1:18000 だけ**・本番と検証用で同じ索引の見込み(推定)・既存の事故の記録は元の Excel で伏せていない見込み(推定)・持ち主は SAT チーム。
- 順番: 個人情報の処理(G1〜G3_SPAN)を共通の前段 → 本 PJ の正本へ commit の後に SAT(本 PJ は書いた後に取り消せない・SAT は消せる)→ 手元の索引(相手側)。

**Why:** これまでの中案 A(決331〜343)は SAT を扱っていなかった。SAT への書き込みは本番に即座に効くので、SAT チームの許可と検証用が先。
**How to apply:** 段 0 = 利用者が `docs/依頼_事故摘録の追加_SATチーム_相手側_顧客への確認_coord_20261104.md` の 3 通を送る → 返事の後に段 1(fed = API と画面・bkb = 工程と Excel とマスク済みの版・dkb = 受入)の req。関連 [[next-phase-challenges-ab]]・[[bedrock-invocation-logging-sandbox]]・[[aws-account-migration-20261031]]。

**決350(2026-11-05 11:23・利用者)**: SAT への書き込みは保留にして、事故摘録の追加の**デモ画面**を作る。書く先 = 正本の写し(s3_harness_dkb の setup --commit の一時 DB・正本の digest 不変を前後で測る)・入力 = デモ用の見本(架空・実在の人名なし・accident_kb_v7/data に入れない)・PDF だけ(Excel は後の段)・まず社内。分担 = fed 画面(既存の intake_console にデモの流れ)・bkb 見本と処理役の設定・dkb 台(kg_api の HTTP・処理役・画面を 127.0.0.1 で 1 命令)と受入。LLM は sat-sandbox・1 USD/件・デモの段の合計 3 USD まで。段 1 = 設計(費用 0・目安 11-07)→ rev → 実装の承認は別に諮る。dec `docs/comms/20261105-1123_coord_to_fed_bkb_dkb_dec_intake-demo-screen.md`・req ×3 同時刻(main e88518018)。


(索引から移した説明・2026-11-07)
- [事故摘録の追加 SAT と本 PJ へ(決349・決350)](intake-accident-sat-registration.md) — 登録先 3 か所(KB・SAT・エージェントの手元の索引)・SAT はマスク済み・**決350 で SAT は保留・デモ画面を正本の写し + 架空の見本 + PDF で作る(段 1 設計 11-07 目安)**
