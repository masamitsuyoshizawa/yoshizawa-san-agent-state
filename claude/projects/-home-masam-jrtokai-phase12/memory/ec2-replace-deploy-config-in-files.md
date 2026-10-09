---
name: ec2-replace-deploy-config-in-files
description: EC2 の kg_api 反映は置換方式(docker cp・退避・タグ・再起動)なので、新しい設定は環境変数や ro マウントでなくイメージ内のファイルで持たせる(T0 の固定値の実例・2026-09-24)
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-24T02:34:59.459Z
---

EC2 の kg_api(`jrtokai-v9-kg-api`)への反映は、動いているコンテナへ `docker cp` で差し替える**置換方式**(退避・`docker commit` の保全タグ・`__pycache__` 削除・`docker restart`)。**`docker restart` では環境変数もマウントも変わらない**ので、新しい口が環境変数(例 `KG_T0_MANIFEST_SHA256`)や ro マウントを前提にすると、コンテナの作り直し(compose の変更)が要り、方式と合わない。

**Why:** 2026-09-24 の W5(`GET /v1/t0/tree`)で、承認計画どおり固定値を環境変数・置き場を ro マウントにしたところ、手順書を書く段で置換方式と合わないと気づき、固定値を `kg_api/kb/config/t0_manifest_sha256.txt`(配る置き場とは別・イメージに COPY される範囲)からも読む形に直した(環境変数があればそちらが勝つ)。置き場は既定のパスに `docker cp`。

**How to apply:** EC2 に反映する新しい設定は、まず「置換方式で置けるか」を見る。環境変数・マウント・ポートの追加はコンテナの作り直し(coord の承認と時刻)になると先に書く。関連: [[ec2-kg-api-deploy-path]] [[verify-what-the-target-actually-reads]]

**実例 2(2026-09-25・D-KB の旗)**: `DKB_SINGULARITIES` は環境変数だけを読んでいたので、dkb が `kg_api/kb/config/dkb_flags.json`(環境変数 > ファイル > 既定・形の誤りは既定のまま uncertainty に 1 行)を足し、coord が `docker cp` で置いて restart・`docker commit` でタグに焼いた(cp だけではコンテナを作り直すと消える)。`.gitignore` に入れてリポジトリには置かない。
