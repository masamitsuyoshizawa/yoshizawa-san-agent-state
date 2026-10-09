---
name: accident-rawdata-api-item
description: 事故摘録の原本を返す API(/v1/accident/rawdata)— 決154〜158 で EC2 反映済み(2026-09-30)・原本は EC2 ホストの読み取り専用マウント・manifest 700 件・A5 アクセスログは前提なし
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-30T09:32:25.840Z
---

顧客(海鉄)の意向で、事故摘録の**受領した原本(PDF 350・xlsx 352 = 702 ファイル・700 事故)**を EC2 に置き API で返す(決154・2026-09-30)。B-KG(DB)は従来どおり伏せた形式を継続。

- 口: `GET /v1/accident/rawdata/{acc_id}`(本体)・`/{acc_id}/{n}`・`/{acc_id}/meta`(masked:false・relpath 無し)。実装 bkb `kg_api/sources/accident_rawdata.py`(是正 2 回後 58aa50748877e66f・コミット c27b5c42)・router 加法 06abd994a5c57b47・試験 102 件は `--base-rev 4d0a89fe^` 必須。
- 契約の読み替えは決154 追補 1〜5(毎回 sha 照合・8 MiB・同時 4・ext 許容集合・O_NONBLOCK)。rev の点検 3 巡(要是正 5 → 要是正 3 → 可)。
- 対応表: dkb `knowledge_kb_v8/scripts/dkg/build_rawdata_manifest.py`・本体ツリー `accident_kb_v7/data/rawdata/`(git 外)・manifest 2478b4119a46da39。acc:0640 は利用者の目視で確定(決156・confirmed.json)。
- EC2: ホスト `/home/ubuntu/jrtokai-v9abc/accident_rawdata` → コンテナ `/app/kg_api/kb/data/accident_rawdata`(ro)+ 環境変数 `KG_ACCIDENT_RAW_DIR`。**マウント追加のためコンテナを作り直した**(道具 `deploy/kg_api_v2/ec2_rawdata_recreate.py`・旧コンテナ `jrtokai-v9-kg-api-pre-rawdata-20260930` は 2026-10-07 まで停止で保持)。manifest を替えたら rsync → `docker restart`。
- 受入 A1〜A4 合格・**A5(アクセスログ)は kg_api にアクセスログが無く前提なし**(決158・現状のまま)。
- R5 完了(fed コミット 549497af): 仕様書 §7(sha16 361a8dd0d51f9dbe・値は合成の原本を実装に当てて採る `spec_extract_rawdata.py`・本物の名と sha256 は 0 件を試験 仕14 で照合)・console(8d702f19e624a1a2・3 口を同じ画面・保存/PDF は開く)は EC2 反映済み(2026-09-30 18:53・タグ rawdata-20260930)。画面の目視は利用者。
- 残り: bkb の送信例外の直接試験(非阻害)・共通表 改訂 16 に記録。

**Why:** 原本(個人名を含む)の配置は顧客判断で承認済みだが、連絡文・コミット・報告に原本のファイル名を書かない規律は続く。
**How to apply:** 原本 API に触るときは決154 の追補まで読む。EC2 の kg_api は compose ではなく docker run で作り直した状態なので、次に env やマウントを変えるときも同じ道具で作り直す。関連 [[facets-instance-entry-issue]] [[ec2-replace-deploy-config-in-files]]
