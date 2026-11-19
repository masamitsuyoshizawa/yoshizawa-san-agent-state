---
name: dify-kgapi-key-in-plugin-credentials
description: Dify のアプリが使う kg_api の鍵は DSL でなく jrtokai_kb プラグインのプロバイダの資格情報(ワークスペースで 1 か所)・EC2 用 D の DSL の api_key は Dify 自身の鍵(2026-10-28 確認)
metadata:
  node_type: memory
  type: reference
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-28T06:20:09.779Z
---

kg_api の鍵(x-api-key)は、Dify の jrtokai_kb プラグインのプロバイダの資格情報にある(`deploy/dify/plugin/jrtokai_kb/provider/jrtokai_kb.yaml` の `credentials_for_provider` の `api_key`・secret-input・`dkg_diagnose.py` は `self.runtime.credentials` から読む)。手元の DSL 5 本(ask・auto_routing・dkg_diagnose・dkg_guided・federate_lite)と EC2 用 D(ec2_diagnose_button.yml)は、tool の欄に api_key を持たず、credential_id も無い(= 既定の資格情報を使う・推定・確度高)。

EC2 用 D の DSL の `api_key` 3 件は、読み直し(http-request)3 節点の認証で `{{#env.DIFY_SELF_API_KEY#}}` = **Dify 自身のアプリの鍵**(kg_api の鍵ではない・決290 G1)。

**How to apply:** kg_api の鍵の交換(決327 など)は DSL を書き換えず、プラグインの資格情報の api_key を 1 か所替える → 各アプリを 1 回ずつ呼んで確かめる → 古い鍵を外す。前に EC2 の DSL を export して tool の欄に鍵が無いことを照らす。関連 [[ec2-dify-dsl-lineage]]・[[dify-plugin-update]]。
