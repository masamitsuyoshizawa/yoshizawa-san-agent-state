---
name: abc-unified-api
description: v1凍結IFに加法でA(文書RAG)/B(事故ベイズ)/C(図表)統合API機能を追加・kg_api単独化・AWS相乗りデプロイ資産まで
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-07-28T03:08:50.266Z
---

A/B/C 統合API機能追加(2026-07-27〜28着手)。目的=v1凍結IFは不変のまま、総合回答エージェントが情報種別・機能を指定して叩く単独APIを、v8以前のデモから分離して kg_api 内に集約。

設計・実装(すべて加法。v1のapp.py削除0行を各段階で確認):
- 追加ルーター(`kg_api/sources/`): docs(A)/accident(B)/federate(A+B+C)。共通エンベロープ`{request_id,source,function,results,answer,meta,warnings}`、認証x-api-key、backend未接続時はavailable:false(v1非干渉)。`/v1/capabilities`追加。
- A=`docs_backend`: V8 retriever(BGE-M3 dense+BM25→RRF→BGE-reranker)+federation DKB。search / diagnose(causes・missing_items(why付)・procedures)。
- B=`accident_backend`: accident_client(9890, accident_ft全文)+accident_diag_engine(決定論ベイズ)。search / clues / diagnose(cause・probability・relevance・support_acc_ids)/ followup(questions:[{id,question,why}] 情報利得で次質問決定)。
- C=既存v1(9990/interlocking)。federateは progress→traceで経緯(A→B→C、B手掛かりをCへ、fuse)を出力。
- **集約(コミット fb9d492)**: モジュール/設定/プロンプトを `kg_api/kb/{scripts,config,prompts}` へvendoring(git追跡)。retriever/federationは`ROOT=parents[1]=kg_api/kb`で index/data/dkb/config を自己解決。大容量・派生・専有データ(`kg_api/kb/index`57M・`kg_api/kb/data/dkb`24M・`kg_api/data/refs_c_index`13M)は.gitignore=デプロイ時配置。config.pyでC→9990統一(`os.environ["KB_NEO4J_URI"]`)。
- 配線: SourceB=`KB_ACCIDENT_NEO4J_URI`(事故9890)、SourceC=`KB_NEO4J_URI`(9990)で混線なし。B診断エンジンはtorch不要、A のみ重量(sentence-transformers+torch+BGE)。

AWS方針(利用者確定 2026-07-28): **現EC2(t3.xlarge/16GB)相乗り+EBS拡張、A+B+C全部含める**。デプロイ資産(コミット cec3258): `deploy/kg_api/Dockerfile.abc`(torch CPU、コードCOPY・索引/DKB/refs/モデルはro bind-mount)、`deploy/docker-compose.v9abc.aws.yml`(project=jrtokai-v9上書き、neo4j-accident追加)、`deploy/env.v9abc.aws.example`、`docs/AWSデプロイrunbook_ABC統合API_20260728.md`。

**デプロイ実行完了(2026-07-28)**: EC2=i-056e80fef788335af / root vol-05f505e2223087e9a を **20→45GiB へオンライン拡張**(modify-volume→growpart→resize2fs、空き1.6G→14G)。コード束(276K, PII無)を `~/jrtokai-v9` へ上乗せ展開、既存 .env の PW/APIキー継承で `.env.v9abc.aws` 生成。索引/DKB/refs は rsync、**モデルは EC2 上で HF から取得**(`~/jrtokai-v9abc/models` 6.4G、HF_HOME=/models 直下=hubサブ無し)。事故グラフは版一致(2026.03.1)で **neo4j-admin dump/load**(scrub済のみ・dump投入後削除・accident_ft継承・store50.5M)。デプロイで判明し修正した集約漏れ(コミット 8c169f7): sounding_eval.py集約・KG_DEPS_DIR=kb/scripts・janome(A BM25日本語)/PyYAML追加。**検証: capabilities全ready、v1=踏切108・ask=4対向(Bedrockフルパス)、A docs/search・diagnose、B accident/diagnose、federate A+B+C(trace6)すべて稼働**。RAM使用8.8G/15G(kg-api RSS2.5G)。kg-apiは **VPC私設IP 172.31.4.27:8600 束縛(公開せずエージェント/VPC内から呼ぶ)**、UIは既存nginx公開のまま。B(PII)はnginx未公開=API直叩きもBasic+APIキー下。

**成果物追加(2026-07-28, コミット 50e3582)**: (1)統合版仕様 `docs/API仕様_KGクエリAPI_統合版_20260728.md`(v1凍結+A/B/C追加8種を1本化)。(2)開発者コンソール `kg_api/dev_console/app.py` に capabilities/docs/accident/federate のフォーム追加(URL `console.54-250-247-97.nip.io` 不変、dev-console再ビルド)。(3)**V10デモ** `kg_api/demo_v10/app.py`(V9と独立・各機能タブ型: A文書RAG/B事故ベイズ対話診断/C図表/横断federate)。streamlit-v10コンテナ(8511)+nginx v10ブロック+acme cert発行で **`https://v10.54-250-247-97.nip.io` 公開(Basic認証下)**。V9デモ既定グラフ不具合は KG_GRAPHS=interlocking,accident で修正済(コミット 64daa5e)。デモ既定=連動、B使用時はサイドバーでaccident選択。

関連 [[v9-api-schema-unification]] [[added-data-integration-2026-07]] [[v8-cross-source-federation]] [[raw-diagram-api]]。
