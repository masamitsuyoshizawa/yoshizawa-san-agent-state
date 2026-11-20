---
name: aws-account-migration-20261031
description: 2026-10-31 に AWS の作業アカウントを sat-sandbox(末尾 4458)から sat-singletenant-jrtokai-dev(末尾 3833)へ替える指示・EC2 2 台(jr-central・jr-central-agent)の引っ越しの調査と手順書
metadata:
  type: project
---

2026-10-31 利用者: saml2aws のアカウントを sat-sandbox から **sat-singletenant-jrtokai-dev / poweruser-role-with-saml** へ替える指示。10/31 中だけは sat-sandbox の費用で LLM 可。新しいアカウントの Bedrock は利用の同意(agreementAvailability)が NOT_AVAILABLE で呼べない見込み → AWS 管理者への依頼文 docs/依頼_AWS管理者_新アカウントの準備_coord_20261031.md。

- 調査 docs/検討_AWSアカウント移行とEC2再構築_coord_20261031.md(改訂 1)・手順書 docs/手順_AWSアカウント移行_EC2の引っ越し_coord_20261031.md(オプション 2 = 2 台を一緒に移すのが推し)。
- 本 PJ の EC2 = jr-central(i-056e80fef788335af・私設 172.31.4.27・公開 54.250.247.97 は EIP でない・EBS 暗号化なし・93% 使用)。エージェント = jr-central-agent(i-035057dacfef12388・172.31.1.45・43.206.157.118)・入口は CloudFront E3GPIYOTTCREU6 → 元 smjr-demo.<IP>.nip.io:7687・WAF jrqa-demo-ip-allowlist。2 台は SG launch-wizard-21 と IAM ロール KbDemoBedrockRole(AmazonBedrockFullAccess だけ)を共用。
- 新旧とも既定の VPC は 172.31.0.0/16 で重なる → ピアリング不可 → 片方だけ移すと通じない。
- 移すと直す所: kg_api の口の結び付け(HostIp 172.31.4.27)・Dify のプラグイン jrtokai_kb の接続先・nip.io の名前 13 と証明書・Dify の .env の URL・エージェントの EXTERNAL_KG_API_URL。
- saml2aws は資格を [default] に書くので、後のログインが上書きする。切り替えは `saml2aws login --force` で一覧から選ぶ。SSH と EC2 の中の LLM はログインに関係しない。

**How to apply:** 移行の作業の各段の頭で STS の末尾 4 桁でアカウントを確かめる。エージェント側の中身は jrtokai-agent-env のセッションに問う(手元の鍵では jr-central-agent に SSH できない)。関連 [[ec2-bedrock-fallback]]・[[api-key-rotation-20261028]]・[[agent-env-repo-kickoff]]。

追記(2026-10-31 23:xx・決345・利用者決定): **新しいアカウントの Bedrock が使えるまで、LLM は sat-sandbox の費用で使う**(11 月以降も)。10/31 の「本日中だけ」は差し替え。資格が切れたら利用者が `saml2aws login --force` で sat-sandbox を選ぶ。同意が済んだら coord が新しいアカウントで 1 回だけ呼んで確かめ、切り替えの時期を知らせる。

追記(2026-11-01・決345 追補 1): 新しいアカウントで最小の呼び出しを実測 = **旧い世代(jp Haiku 4.5・jp Sonnet 4.5/4.6・global Opus 4.6)は呼べ、新しい世代(jp Opus 4.7/4.8/5.5・global Sonnet 5/5.5・jp Haiku 5.5)は AccessDeniedException「not available for this account … contact AWS Sales」**。get-foundation-model-availability の表示はどちらも同じ(agreement NOT_AVAILABLE 等)で**あてにならない**。確かめるなら最小の呼び出し(ping・maxTokens 5)で。依頼文は改訂 2 で新しい世代の許可を具体的に頼む形にした。

追記(2026-11-01・決345 追補 2): 新しいアカウントの OpenAI = 東京の on-demand の gpt-oss-120b/20b・safeguard-120b/20b は呼べる(地域の中)・GPT-5.4〜6.1 は global の推論プロファイルだけで全部 AccessDenied(not available for this account)。OpenAI に jp の推論プロファイルは無い(国内処理の決定とは合わない)。

**決348(2026-11-03〜04)= 複製は済み・旧は無停止**: 再起動なしの AMI → 新(…3833)で旧と同じ私設の番地(172.31.4.27・172.31.1.45・1c)で起動。jr-central = i-0b7ee3f6579a976ce・EIP 57.181.3.232(`*.57-181-3-232.nip.io`)、agent = i-0f39106eb760b0e70・EIP 46.51.236.30(`smjr-*.46-51-236-30.nip.io`)。SG は利用者の PC の IP だけ。4 DB の digest・kg_api 15 口・画面 14 名・エージェント 5 処理が旧と同じ。Bedrock は新世代が拒否のまま(Haiku 4.5・Sonnet 4.6 は可)。手順書 改訂 2 §0 に、切り替えの時に要ること(Bedrock の許可・データの取り直し・SG・CloudFront・周知)。
**旧の危険(利用者へ報告)**: jr-central の nginx の設定はコンテナの中だけ(ディスクは 9/4 の古い版で、作り直すと neo4j-v4 で起動しない)・エージェントの le.crt が 2026-11-16 に切れる(手で renew_cert.sh)。複製のエージェントは Azure の資格を本番と共有(書き込みをしない)。

追記(2026-11-05 17:18・利用者「申請済みの連絡を受けた」): 新しいアカウントで再び実測 = **変わらず**。東京・大阪で jp Sonnet 4.6・jp Haiku 4.5 は呼べ、global Opus 5/5.5・global Sonnet 5・jp Opus 4.7/4.8/5.5 は AccessDenied(not available for this account)。us-east-1 の global/us も同じ。get_foundation_model_availability(東京)= 新しい世代は **agreementAvailability NOT_AVAILABLE**(authorization AUTHORIZED・entitlement AVAILABLE・region AVAILABLE)、Sonnet 4.6 は agreement AVAILABLE。= 足りないのは利用の同意だけ。報告の md のアクション E を「未完了」に更新。


追記(2026-11-10 10:01・決345 追補 3・利用者が新アカウントにログイン): 再実測 = 新しい世代は変わらず AccessDenied。呼べるのは jp Haiku 4.5 と jp Sonnet 4.6 だけ(東京・大阪)。**11-01 に呼べた jp Sonnet 4.5・global Opus 4.6 も今は ResourceNotFoundException**(東京 = use case details not submitted・大阪 Sonnet 4.5 = Legacy)。agreement が AVAILABLE なのは Haiku 4.5・Sonnet 4.6 だけ。LLM は sat-sandbox のまま。手元の [default] が新アカウントになっているので、LLM の作業の前に sat-sandbox へ入り直しが要る。

(索引から移した説明・2026-11-07)
- [AWS アカウント移行(20261031)](aws-account-migration-20261031.md) — sat-sandbox → sat-singletenant-jrtokai-dev・EC2 2 台(jr-central・jr-central-agent)は一緒に移すのが推し・新アカウントは旧い世代だけ呼べ Opus 4.7/5.5・Sonnet 5 は AccessDenied(可否の API はあてにならない)・それまで LLM は sat-sandbox(決345)・**決348 で 2 台を新へ複製済み(旧は無停止・切り替えは利用者の指示待ち)**


(索引から移した説明・2026-11-20)
- [AWS アカウント移行(20261031)](aws-account-migration-20261031.md) — 決348 で EC2 2 台を新へ複製済み(切り替えは利用者の指示待ち)・新は Opus 4.7/5.5 が AccessDenied・LLM は sat-sandbox(決345)
