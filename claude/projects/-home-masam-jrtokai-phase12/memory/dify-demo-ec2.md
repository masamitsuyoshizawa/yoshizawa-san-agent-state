---
name: dify-demo-ec2
description: EC2にDify CE 1.16.1構築済み・KB API連携デモ2本稼働(ask/federate-lite)
metadata: 
  node_type: memory
  type: project
  originSessionId: c2841d1b-d6c4-4a43-ac52-21bc3fbfca51
  modified: 2026-08-19T06:35:35.268Z
---

2026-08-17完了: kb-demo-ec2 の `~/dify` に Dify CE 1.16.1(15コンテナ、プロジェクト名dify)を構築。https://dify.54-250-247-97.nip.io/ で公開(LE実証明書・acme.sh自動更新・demo-nginx vhost経由→172.31.4.27:8081)。デモ2本=「連動図表・踏切制御図表ナレッジ・デモ」(ask、約40-50秒)/「事故摘録ナレッジ(1問1答式)・デモ(約2分)」(federate/lite、sources=A,B・mode=fused・fuse_lead=A・k=6)。**自作ツールプラグイン jrtokai_kb 経由**(ソース=deploy/dify/plugin/jrtokai_kb、FORCE_VERIFYING_SIGNATURE=false緩和、dify CLI 0.6.10でパッケージ、資格情報にAPIキー一元管理)。公開実行URL(ログイン不要): /chat/vMfHq4ezqUhRjM98 と /workflow/gPSZ7kyiyEI2ExVU。EBSは45→64GiBへ拡張済(ローカルのpoweruser SAMLロールでmodify-volume可能)。

**Why:** 顧客向けオーケストレーションデモ。既存サービスと独立に起動/停止可(`cd ~/dify && docker compose up -d / stop`)。

**How to apply:**
- 管理者認証情報=EC2の `~/dify/ADMIN_CREDENTIALS.txt`(chmod600・非コミット)。ログインAPIはパスワードBase64送信・Cookie認証(access_token/csrf_token)。
- kg_apiへのHTTPノード到達は `.env` の `SSRF_PROXY_ALLOW_PRIVATE_IPS=172.31.4.27/32` が前提。タイムアウトは `HTTP_REQUEST_MAX_READ_TIMEOUT=300`。
- DSL正本=リポジトリ `deploy/dify/dsl/`(キーは `__KG_API_KEY__` プレースホルダ、インポート時のみ置換)。手順書=`deploy/dify/構築手順書_dify_20260817.md`。
- 3本目「ナレッジ自動仕分けデモ」(20260818): 質問分類器ノード(Claude Sonnet 5)で図表KB/事故摘録へLLM振り分け。公開URL=/chat/xxF7Kl1dnQ1fGJe3。DSL=dsl/auto_routing_chatflow.yml。
- v0.2.0(20260818): 両ツールにprovider select(11モデル+default)。kg_apiのモデル増減時はツールYAML+DSL開始ノードのoptionsを更新しversionを上げる。
- LLMプロバイダ(20260818): bedrock0.0.79=IAMロール認証(ap-northeast-1)・vertex_ai0.0.61=SA(project=paas-sat-sandbox/location=global、kg_apiコンテナ内ADCから非表示転記)。既定LLM=anthropic claude 5。GPT-5.6系は404だったが**ローカルパッチ版で解消済み**(converse+globalプロファイル経路へ3箇所修正、bedrock_plugin_gpt56_patch.md参照。使用時はcross-region=global必須。マーケットプレイス更新でパッチ消失→再パッチ要。モデルプロバイダ資格情報はプラグイン入替で消える=要再登録)。
- DSL上書きインポート後は利用者に**編集画面のタブを全て閉じて開き直す**よう依頼する(リロードだけでは不十分: 別タブが共同編集リーダー(Redis workflow_leader:<app_id>、TTL約1h)として残ると旧グラフが再配信される。20260817/0819に実発生)。サーバ側解消=Redisキー削除+api_websocket再起動+DSL再インポート。question-classifierのclassesにはlabel(文字列)必須。
- console APIのログインはパスワードBase64+Cookie認証。プラグイン更新はversion上げ→upload/pkg→install/pkg→tasks確認。資格情報は/tool-provider/builtin/{provider}/add(type=api-key)。
- 関連: [[ec2-kg-api-deploy-path]] [[federate-latency-optimization]]
