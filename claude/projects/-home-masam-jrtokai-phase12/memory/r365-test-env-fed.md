---
name: r365-test-env-fed
description: 決365(2026-11-23 承認)試験環境と v2 の計画での fed の持ち分 = 段 1 の新旧の答えの形の比べ・段 5 の統合の口の追随と一括追加の窓口・段 6 の切り替え。用意した物と、事故 ID を使う所の数え
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-11-23T08:16:32.630Z
---

決365(dec 20261123-1710・計画 `docs/計画_決365_試験環境とv2_coord_20261123.md` 改訂 0)= 今の EC2 はそのまま、新しいアカウント(末尾 3833)の複製の 2 台を試験環境にして、v1 の写しで比べの基準 → 伏せ → B-KB v2 → 新規の追加 → D-KB v2 と口 → 切り替え。切り替えは「今の EC2 を入口として残し、向き先だけを替える」(dkb の推し・呼ぶ側の URL は不変)。

- fed の持ち分: 段 1(新旧の答えの形の比べ・dkb・bkb・ckb と)・段 4(新規の追加の窓口)・段 5(統合の口の追随・一括追加の窓口を有効に)・段 6(切り替え)。
- 2026-11-23 17:16 に用意した物(実装はしない・coord の指示): 比べの一覧の案 `docs/設計案_新旧の答えの形の比べの一覧_fed_20261123.md`(sha16 d51a81f8d63d955c・api_validate.py に --manifest と --old/--new を足す案)・dkb へ形の突き合わせの依頼(req 20261123-1716)・事故 ID を使う所の数え(rep 20261123-1716)。
- 2026-11-23 17:24 改訂 1(sha16 d61ec60ed5d1e0cc・dkb rep 20261123-1723 を入れた): 先頭に schema・行の欄 tie(同点の並び)と expect_diff(段ごとの予定の差・docs/eval/compare_expected_<段>.json)・D と A の口は LLM 0(dkb が確定)・道具は api_validate.py を変えず別の docs/eval/api_compare.py(HTTP と鍵は import)。docs_router・docs_backend・t0_router の持ち主 = fed(決365 追補 2・t0 の snapshot の中身と作り手は dkb = 中身の期待は dkb に尋ねる)。`dkg_tree.py` は fed の持ち分。
- 2026-11-23 21:19 段 0 の後、段 1 の fed の設計の表 `docs/設計_決365_段1_答えの比べと入口_fed_20261123.md`(sha16 eef3a0ea6d1f805b・rep 20261123-2119): 比べは新旧とも ssh の口の写しで kg_api に直に・LLM を呼ばない口 69 × 2 回り(今の EC2 に 138)・LLM の口 9・MCP 8610 に 10・MCP-UNI と UNI2 は今の EC2 に当てない(QA の記録と会話の DB に書く既定)。MCP の像は docker save を手元を通して流す。Dify の DSL と .env に 172.31.4.27 の決め打ち。今の EC2 の MCP は kg_api を nginx を通さず直に呼ぶ(入口の向き先を替えても追随しない = 段 6)。dkb へ分担の req 20261123-2119 → dkb rep 2120 で合った → 改訂 1(sha16 7c3e71df89cbf88c・info 20261123-2122): 試験環境の kg_api = 今の EC2 と同じ私的な番地の 8600(決348)・担当は「用意 fed / 打つ coord」・秘密は dkb の素通しの管(利用者に諮る)・Bedrock は dkb の案 A(MCP-UNI 8eb8202 と UNI2 e37ee8f は boto3 の既定の探し方 = 確定)・借りる役割の許しに UNI と UNI2 のモデルと大阪を含める事を諮る。実装は承認の後。
- 2026-11-23 21:34 〔改訂 2〕(sha16 611f7d4b1c004fac・rev rep 2127 = 要是正・rep 20261123-2134): 今の EC2 には読みの要求だけ(書きの拒みは複製だけ・上限 144)・LLM の口を今の EC2 に当てない(推し・選び)・複製の側の LLM の口も実の資料を 4458 へ送る = 在る承認で覆えるかを諮る・meta.llm_calls の欠けを 0 としない・MCP の秘密は試験専用に作る・段 1 は SAT を止める・呼ぶモデル = UNI global.anthropic.claude-sonnet-5-5・UNI2 jp.anthropic.claude-haiku-4-5-20251001-v1:0・Dify Sonnet 5 global(推定)・8610 は呼ばない。**教訓: 今の側(本番)に送る物は、拒まれる作りでも書きの要求を入れない。合成の問いでも、モデルへの最終の入力(取った実の資料)で判じる。**
- 事故 ID の新しい体系(ACCR-yyyymmdd-nnnnnn の案・計画 §5)で壊れる形の照らし = MCP `normalizers.py` の数字だけ・MCP-UNI `router.py` の 1〜5 桁・kg_api `accident_rawdata.py` の 4 桁・`dkg_tree.py` の 4 桁。MCP の道具の説明と指示の文に acc:NNNN の例(変えるならプロンプトの変更 = 人の承認)。I4(今の欄は今の形のまま・新しい ID は欄で足す)なら壊れない。

**Why:** 段 0(新アカウントの 2 台の下見)の後、段 1 から coord が実装を頼む。その時に、この用意と数えから始める。

**How to apply:** 段 1 の依頼が来たら、dkb の答え(比べの一覧の形)を先に読む。比べの道具は値を出さない(違った欄の道と数だけ)。MCP のソースは `~/jrtokai-mcp-server`(git の管理・MCP の担当の物)。関連 [[ec2-inplace-replace-stopped-test-env-direction]] / [[mcp-server-ec2-cloudfront-r359]] / [[storage-order-changes-dkb-responses]]
