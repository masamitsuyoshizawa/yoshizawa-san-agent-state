---
name: v9-api-schema-unification
description: V9計画=API I/F凍結・スキーマ日本語正準統一・薄クライアントUI・検証環境・AWSコンテナ化の進捗と設計
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
---

V9 計画(2026-07-15 着手)。計画書 `~/.claude/plans/fuzzy-exploring-corbato.md`。目的=路線間プロパティ名の統一・API呼び出し前提のデモUI・検証環境・AWS公開(I/F凍結でローカル→AWS随時反映)。

進捗(すべてローカル完了・コミット済):
- Phase 1 API I/F凍結: `docs/API仕様_KGクエリAPI_v1_凍結_20260715.md`(実装準拠。10エンドポイント・エンベロープ・meta実キー・errorネスト形状・422はFastAPI既定)。以後I/F不変。
- Phase 2 スキーマ統一(**加法正規化・日本語キーが正**): `kb_demo_v6/normalize_props.py`(冪等・9990のみ・9790不変・LLM不使用)で衝突7ラベルに正準日本語キー7,726件付与。決定=種別/保安装置形式に分離(crossing_typeが路線で別概念)・所属駅は駅名統一(飯田線コード→駅名)。語彙定義=`docs/正準プロパティ語彙_C-KG_案_20260715.md`。新路線は投入後 normalize_props.py 実行で統一。kansai既存プロパティは不変=208回帰は変化なし(飯田線は208スコープ外)。
- Phase 3 V9デモUI: `kg_api/demo_v9/app.py`(Streamlit薄クライアント。kg_apiのHTTPのみ、Neo4j/LLM非依存)。
- Phase 4 検証環境: `docs/eval/api_validate.py`(全10エンドポイントの凍結IF適合スモーク。--baseでAWSにも向け契約一致確認。ローカル16/16 PASS)。judge_eval.pyは KG_API_BASE/KEY/KG_EVAL_GRAPH で可変化。
- Phase 5 AWSコンテナ化: `deploy/kg_api/Dockerfile`(実ビルド→凍結IF13/13・**refs無効で98MiB**)、`deploy/streamlit-v9/Dockerfile`、`deploy/docker-compose.v9.aws.yml`、`docs/AWS反映runbook_V9_20260715.md`。app.py内部改修(I/F非変更)=パス/接続URI環境変数化(KG_DEPS_DIR/KG_REFS_DIR/KG_INTERLOCKING_URI等)・refsトグル(KG_ENABLE_REFS)。**refsのdense(BGE-M3/torch)がRAM+2〜4GBの唯一の重要因**、既定はtorch非導入=lex-onlyで軽量。

Phase 6 AWSデプロイ完了(2026-07-15): 現EC2=**t3.xlarge/16GB**(RAM余裕、ディスクが制約→旧v3スタック撤去で19GB中空き2.6→5.3GB)。EC2はgit管理外(Apr tarball)のため**最小V9バンドル(v9-deploy-bundle.tar.gz、PII無)をscp→`~/jrtokai-v9`展開**。専用プロジェクト名 `jrtokai-v9`(既存本番と隔離)。Cypherダンプ(cgraph_full.cypher, cypher-shell -f, 9990停止不要)で4790/9293投入。**凍結IF検証 api_validate 16/16 PASS(Bedrock=KbDemoBedrockRoleインスタンスロールで nl2cypher/query/ask 成立)**。refs=lex-only(torch非導入=軽量、kg_api約0.1〜0.4GB)。**一般公開まで完了(2026-07-15)**: `https://v9.54-250-247-97.nip.io`(Basic認証 jrtokaiユーザ)。既存nginx(`deploy/nginx-new/nginx.conf`, イメージ焼込)にv9 server block追加+撤去済v3ブロック削除、`jrtokai-v9-streamlit`を共有ネット`deploy_jrtokai-net`へ`network connect`、cert=acme.sh手動issue(`/certs/streamlit-v9/`)、`nginx -t`同ネット事前検証→再作成でv9/v7とも health=ok。nginx.confはリポジトリにも反映済。反映は git push 済(stockmarkteam/yoshizawa-san)。V9計画=全フェーズ+AWS公開まで完遂。

関連 [[added-data-integration-2026-07]] [[raw-diagram-api]]。
