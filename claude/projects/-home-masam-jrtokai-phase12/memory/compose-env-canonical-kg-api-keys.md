---
name: compose-env-canonical-kg-api-keys
description: EC2 の compose の正本(~/jrtokai-v9/.env・deploy/.env.v9*.aws)は旧鍵 1 つだけで、compose は Streamlit の KG_API_KEY にも ${KG_API_KEYS} を当てていた。決330-2 で 3 鍵 + KG_API_KEY_INTERNAL に是正(2026-10-28)
metadata:
  type: project
---

2026-10-28 の鍵の交換(決327 段 A・決329 段 B)は、稼働中のコンテナの env だけを inspect ベースの道具で替えた。EC2 の compose の正本(`~/jrtokai-v9/.env`・`deploy/.env.v9.aws`・`deploy/.env.v9abc.aws`)は `KG_API_KEYS` に旧鍵 1 つ(識別子 56b06f34)のままで、compose(`deploy/docker-compose.v9.aws.yml`・`v9abc.aws.yml`)は Streamlit/dev-console の `KG_API_KEY` にも `${KG_API_KEYS}` を当てていた。

**Why:** compose から作り直すと kg_api が旧鍵だけに戻り、本 PJ の 6 コンテナ・Dify(87e56d75)と外部用の新鍵(cdec15eb)が 401 になる。鍵の組が複数になった時点で `KG_API_KEY: ${KG_API_KEYS}` は壊れる(1 鍵のときだけ偶然成立していた)。分析セッションの依頼(項目 4)で気づいた。

**How to apply:** 決330-2 で是正 = git の compose 2 本は `KG_API_KEY: ${KG_API_KEY_INTERNAL}`(main c315d3c56)・EC2 の .env 3 本に `KG_API_KEYS=3 鍵` と `KG_API_KEY_INTERNAL=本 PJ 用鍵`(0600・.bak-20261028-20xx)・EC2 の compose 2 本も同じ置換(EC2 版は git と差分があるので全体は上書きしない)。**コンテナの env を替えたら、同じ便で compose の正本(.env)も替える。** 道具 = EC2 `~/t327/env_canonical_update.py`(値は inspect から取りファイルの中だけに書く・表示は sha8)。EC2 の compose と git の compose は別物(EC2 用の最小差分)なので、照合は sha16 でなく該当行で。関連 [[api-key-rotation-20261028]] [[ec2-replace-deploy-config-in-files]]
