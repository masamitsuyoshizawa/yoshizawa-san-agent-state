---
name: ec2-bedrock-fallback
description: saml2aws失効時のLLM実行代替=EC2ホスト(IAMロール)+大阪リージョンbedrock・手順は/home/ubuntu/llmwork
metadata:
  type: project
---

saml2aws 2段階認証が使えないときのLLM抽出の一時手段(2026-09-08確立):
EC2ホスト(kb-demo-ec2)のIAMロール KbDemoBedrockRole で bedrock を直接呼ぶ。
制約2点(実査済): ①東京リージョン(ap-northeast-1)のbedrock-runtimeはEC2から接続不能(タイムアウト)
→ **ap-northeast-3(大阪)経由**なら同じ jp.anthropic プロファイルが動く。②コンテナ内からは
IMDSホップ制限で認証取得不可 → **ホスト上で実行**。boto3はユーザ領域に更新済み
(pip3 install --user --break-system-packages)。converseの temperature は廃止(渡すとValidationException)。

**Why:** リモート作業時にローカルsaml2awsの2FAが使えない事態が発生したため。
2026-09-08追記: **EC2のkg_api本体のLLMも同原因で不動作だった**→llm_providersに
KG_BEDROCK_REGION 環境変数上書きを実装し、composeのkg_api envに ap-northeast-3 を設定して恒久解決
(コンテナからIMDS認証は取得可=ホップ制限ではなかった)。
**重大教訓: EC2で `docker compose up` するとコンテナ層のdocker cp蓄積が全て消える**(ビルドイメージが古い)。
復旧= docker tag <最新保全タグ> jrtokai-v9-kg_api:latest → compose up --no-build。以後、保全commitの度に
compose用latestタグ(kg_api/streamlit-shirei/streamlit-v10)も更新する運用とする。
Dify(2026-09-08): plugin_daemonも同遮断→Bedrockプロバイダ資格情報を us-east-1 の新credentialで作成し
switch APIで有効化(プラグイン0.0.79のリージョン選択肢に大阪なし・globalプロファイル前提。手順=構築手順書§8)。
admin資格情報はEC2 ~/dify/ADMIN_CREDENTIALS.txt(値は会話に出さない)。
**How to apply:** 抽出専用スクリプト+チャンクを /home/ubuntu/llmwork へscp→ ssh -f + setsid nohup で
デタッチ起動(sshの&は残骸bashが残るので注意・пgrepの自己マッチにも注意)→jsonl回収→ローカル投入。
resume型で書くこと(二重起動で重複行が入り得る→chunk_idでuniqしてから投入)。

## 追記(2026-10-09)
- 代替 PC では `saml2aws login` の結果が `~/.aws/credentials` の **[saml]** に入り、**[default] は失効のまま残る**ことがある(10/09 に default 10:57 失効・saml 20:59 まで有効)。Bedrock を呼ぶ道具は `AWS_PROFILE=saml` を明示する(aws sts get-caller-identity --profile saml で確かめてから)。
- saml2aws の role 自動選択の設定(role_arn・skip_prompt)は無い。role 選択の繰り返し失敗は設定側ではなく応答側の問題と推定(2026-10-09・未解明)。
- 手元が失効している間の S6 (c) は EC2 ホストの IAM ロール(大阪・global Sonnet 5)で回せた(bkb・約 1.07 USD/24 件)。
