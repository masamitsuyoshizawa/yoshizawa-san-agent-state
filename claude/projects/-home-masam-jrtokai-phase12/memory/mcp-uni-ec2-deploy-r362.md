---
name: mcp-uni-ec2-deploy-r362
description: 決362(2026-11-19): 統合の MCP サーバー MCP-UNI(jrtokai-mcp-uni・8620・/mcp-uni)を EC2 kb-demo-ec2 に今の MCP と並行で配備した・構成と戻し方と気になる点
metadata:
  type: project
---

利用者の依頼(MCP の担当の依頼書を貼って)と 3 つの選び(今の MCP の鍵を共用・SAT のテスト環境のマニュアル 18 件の入れ物だけ有効・既定は Sonnet 5.5 global)で、2026-11-19 00:30〜00:40 に配備。確かめの 7 点は全部通った。報告 = `docs/報告_MCP-UNIのEC2並行配備_結果_coord_20261119.md`・dec = 20261119-0030(決362・追補 1)。

- EC2: `~/mcp-uni/`(束 mcp_uni_src_v020.tar sha16 f636eee61ea1d84c = git archive v0.2.0-uni c7bb97e・道具 uni_deploy1.sh / uni_env.py / uni_run1.sh / nginx2.py・`secret/uni.env` 0600)。像 jrtokai-mcp-uni:0.2.0-20261118(312 MB・UID 1000)。容器 jrtokai-mcp-uni = 172.31.4.27:8620。記録の置き場 /home/ubuntu/jrtokai-mcp-uni-data(0700)→ /data(SQLite)。
- nginx: MCP 用の vhost に `= /health-uni` と `/mcp-uni` を足した(sha16 c1b72aa5e4fe6e14 → 85be7dd7d3052c6c・退避 `~/nginx_backup/nginx.conf.pre-dec362-20261119`)。CloudFront・WAF・SG は不変。
- 設定: Bedrock は EC2 の役割 KbDemoBedrockRole・リージョン ap-northeast-3(kg_api と同じ・依頼書の例は東京だった)。署名の秘密は MCP-UNI 専用。kg_api の鍵は UID 1000 が読める方(~/mcp/secret/kg_api.key)。

**Why:** 顧客の Kiro から、1 つの窓口(uni_ask)で、kg_api の 5 DB と SAT のマニュアルを引いて回答を合成させるため(MCP の担当の作り)。
**How to apply:** 戻し = nginx を退避から戻して reload → `docker rm -f jrtokai-mcp-uni`。入れ物の ID は、エージェントの EC2 の `~/sat-jrc-agent-staging/environments/staging/.env` から読んだ(手元に無い・スクラッチパッドの agent_ssh.sh = EC2 Instance Connect・AWS のログインが要る)。気になる点(MCP の担当へ戻した): 承認前の QA の記録を鍵を持つ人の全員が読める・消す口なし・`UNI_MCP_SERVER_API_KEY_FILE` が効かず鍵 0 個で素通り・費用の上限なし・問いの文が伏せなしで SAT と Bedrock へ・既定の出力が上限 1,800 に当たる。EC2 の B は伏せ直しの前なので、返る事故の本文も前の版。関連 [[mcp-server-ec2-cloudfront-r359]] [[sat-api-sandbox-check-fed]] [[r351-round1-apply-commit-gate]]

**2026-11-19 昼(決362 追補 2・3)**: 像を v0.2.1-uni(コミット f0b9503・像 jrtokai-mcp-uni:0.2.1-20261119)に入れ替えた。確かめ 4 点は通った。**依頼書と違う点 = 設定に UNI_SCRUB_OUTBOUND_QUERY=false を足した**(利用者の決め。問いの伏せの規則が「同様」「仕様」「お客様」などを伏せ、kg_api の検索にも使われるため。規則を直した版が出たら、この行を外す事を諮る)。承認前の記録は消していない(利用者の決め・入れ替えの前に 9 件 = coord の 2 + ほかの方の 7)。purge は確かめなしで全部を消すので、打たない。前の像 0.2.0 は消した(戻す時は EC2 の束から作り直す)。報告 = `docs/報告_MCP-UNIのv0.2.1入れ替え_結果_coord_20261119.md`。入れ替えの型 = 像を作る → 記録の写しと今の設定で、口を出さない一時の容器 → 前の容器の名を替えて残す → 確かめ → 消す。

**2026-11-19 午後(決362 追補 4・5)**: 像を v0.2.2-uni(コミット 8eb8202・像 jrtokai-mcp-uni:0.2.2-20261119)に入れ替えた。確かめ 4 点は通った。**伏せの 1 行(UNI_SCRUB_OUTBOUND_QUERY=false)は消した**(利用者の決め = 依頼書どおり有効に)。設定は 0.2.0 の時と同じ bytes。ただし規則には、空白で区切った形の誤伏せ(13 個中 10)と、伏せ漏れ(19 の形のうち 12)が残る(担当へ戻した)。0.2.2 では kg_api への照会は伏せていない問い。vacuum 済み(auto_vacuum = INCREMENTAL)。前の像 0.2.1 は消した。報告 = `docs/報告_MCP-UNIのv0.2.2入れ替え_結果_coord_20261119.md`(写しを依頼書の隣に)。教訓: 作り手の「直した」は、文の形の例だけで確かめずに、**形を変えた例(空白の区切り・ラベル・職名の並び)と、反対の側(伏せ漏れ)** も当てる。

**2026-11-19 深夜(決362 追補 6・7)**: 3 つ目のサーバー **MCP-UNI2**(コンテナ jrtokai-mcp-uni2・像 jrtokai-mcp-uni2:0.3.0-20261119・タグ v0.3.0-uni2 fce0c21・口 8630・/mcp-uni2・/health-uni2)を並行で配備した。確かめ 6 点は通った。サーバーの中で Bedrock の Tool-Use のループ(最大 4 ラウンド)を回し、kg_api の 9 道具 + SAT の 2 道具を AI が選ぶ作り。設定 `~/mcp-uni2/secret/uni2.env`(下流の鍵は共用・署名の秘密は新しい乱数)・記録 `/home/ubuntu/jrtokai-mcp-uni2-data`。nginx の設定 sha16 は 85be7dd7d3052c6c → aa50c8c13b2f98f4(退避 `~/nginx_backup/nginx.conf.pre-dec362-uni2-20261119`)。**外に面する容器は 3 つ**(決351 の EC2 の作業の窓で止めて戻す)。気づいた事: 1 問が重い(入力約 2.1 万・出力約 3,900 トークン)・モデルの制限なし・ログに内部の URL・session に問いと回答の抜粋。報告 = `docs/報告_MCP-UNI2のEC2並行配備_結果_coord_20261119.md`。教訓: nginx の設定に在る番号の字(8630)は、既存の容器の中の口でも当たるので、「既に在るか」は番地つきで見る・名ごとの応答の前後比べは、鍵がつぶれていないかを見る。

**2026-11-20 未明(決362 追補 8)**: MCP-UNI2 を v0.3.1-uni2(e37ee8f・像 jrtokai-mcp-uni2:0.3.1-20261120)に入れ替えた。**既定のモデルは claude-haiku-4-5**(設定は UNI2_DEFAULT_MODEL の行・UNI_DEFAULT_MODEL は uni2 に効かなくなった)。確かめ 3 点は通った。ログの内部の URL の行は 0 になった。残り = 選べるモデルの制限なし(7 つ)・session の保存は既定で有効。報告 = `docs/報告_MCP-UNI2のv0.3.1入れ替え_結果_coord_20261120.md`。
