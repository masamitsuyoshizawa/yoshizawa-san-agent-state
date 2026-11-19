---
name: api-key-rotation-20261028
description: 決327(2026-10-28): 本 PJ API 鍵の交換・段 A 完了(旧 + 新内部 + 新外部の 3 鍵を kg_api に並べた)・段 B(本 PJ の 6 コンテナ + Dify DSL 4 本の切替)と段 C(旧鍵の撤去)は別 dec・鍵の値は ~/.kb_*_key_20261028
metadata:
  type: project
---

外部エージェント側の EC2 で本 PJ の API 鍵の値が .bash_history に残っていた(jrtokai-agent-env の分析・2026-10-28)→ 利用者決定 決327 = 交換し外部専用の鍵を分ける。

- 事実(確定): EC2 kg_api の `KG_API_KEYS` は 1 つだけだった(sha16 先頭 56b06f34)。本 PJ の 6 コンテナ(Streamlit v10/shirei/t0/facets-v3/v9・dev console)の `KG_API_KEY` と Dify の D の DSL の tool `api_key`(3 箇所)も同じ。外部エージェントは nginx を経由せず 172.31.4.27:8600 を直接呼ぶ(nginx 14 日に外部の口の要求 0)・uvicorn は log-level warning でアクセスログ無し。
- **段 A 完了(2026-10-28 06:13 UTC)**: `~/t327/kgapi_rotate_add.py`(EC2・inspect から同じ設定・KG_API_KEYS だけ置換)で kg_api を作り直し = 鍵 3 つ(旧 56b06f34・新内部 87e56d75・新外部 cdec15eb)・像 t321 不変・各鍵 200・無関係 401。**鍵の値はこの PC の `~/.kb_external_key_20261028`・`~/.kb_internal_key_20261028`(0600)だけ**。外部用は利用者が相手に渡す。
- 残り: **段 B** = 本 PJ の 6 コンテナ(recreate_generic.py で env の KG_API_KEY を置換)と Dify の DSL 4 本(D/G/ask/federate-lite・tool の api_key・fed に手順を相談)を新内部鍵へ(別 dec)/ **段 C** = 相手の 4 系統の切替完了後に旧鍵を外す(kgapi_rotate_add の逆・別 dec)/ 別件 = kg_api のアクセスログ(口・状態・所要・鍵の識別子)の追加。
- 分析セッションへの回答 = `~/jrtokai-agent-env/docs/回答_本PJcoord_外部エージェントの件_20261028.md`(依頼 1〜6)。

**How to apply:** 段 B の前に Dify の tool 設定の替え方(DSL の api_key を書き換えて import か console の tool 設定か)を fed に確認。段 C は相手の報告(利用者経由)を待つ。鍵の値を連絡文・チャット・コマンド行に書かない。関連 [[agent-env-repo-kickoff]]・[[ec2-replace-deploy-config-in-files]]。
- **決328(2026-10-28 15:5x)**: kg_api にアクセスログを足した(coord 実装・main 82a6e714a・kg_api/sources/access_log.py ff4f38fb・app.py の try 組み込み・試験 test_access_log.py T1〜T8)= 1 行 JSON {ts,method,path,status,ms,key=sha256 先頭 8}・本文/IP/鍵の値なし・healthz 除く・KG_ACCESS_LOG=0 で停止。rev R328・bkb 受入の後、**EC2 は段 B と同便で反映**(像の作り直し・反映後に依頼 5 の実績を鍵ごとに集計できる)。
- fed の下調べ(確定): Dify の DSL 5 本の tool に kg_api の鍵は無い・鍵はプラグインのプロバイダ資格情報 1 箇所(console の口の形は段 B の準備で確かめる)。
- **決329(2026-10-28 16:5x)**: 段 B は rev R328 合格後に coord が自動で実施(改めて確認しない)= (1) kg_api にアクセスログ(像 t328・~/t327/kgapi_t328.sh)(2) Dify 資格情報 update(provider id `jrtokai/jrtokai_kb/jrtokai_kb`・credential 1 件 name kg_api・~/t327/dify_cred_update.py)(3) 6 コンテナを recreate_env.py で新内部鍵へ(~/t327/stage_b_consumers.sh)(4) 1 日ログを見て段 C。EC2 ~/t327/key_internal(0600)は実施後に shred。
- **段 B 完了 2026-10-28 19:3x(決329 追補 2)**: kg_api 像 t328-20261028(アクセスログ ON・鍵 3 つ)・Dify 資格情報 = 新内部鍵・6 コンテナ = 新内部鍵(nginx は container 名 + 8509〜8514 で到達)。鍵ファイルは shred 済み。**残り = 段 C**(相手の切替完了の後・旧鍵 56b06f34 を KG_API_KEYS から外して作り直し・観察は `docker logs --since 24h jrtokai-v9-kg-api | grep 56b06f34`)。R328-T8(試験 T8 の補強)は別コミット。
- **決330(2026-10-28 20:11)**: 旧鍵 56b06f34 は期限なく残す(段 C は相手の切替完了 + ログで旧鍵 0 + 利用者 dec の 3 条件)・分析側が呼ぶなら本 PJ 用 87e56d75・鍵ファイル 2 つは kb_backup.sh の S に入れた(F: の gpg 退避)・compose の正本は [[compose-env-canonical-kg-api-keys]]。見張り = EC2 `bash ~/t327/access_log_watch.sh 24h`(読み取りのみ)。

**決341(2026-10-30・利用者の依頼)**: 4 つ目の鍵 **f1fa83e3** を追加(既存 3 鍵は残す)。値は手元 `~/.kb_external_key2_20261030`(600)だけ・kb_backup.sh の S に追加。EC2 は kg_api を recreate_env.py で作り直し(退避の像 pre-key4-20261030)+ compose の正本の .env 3 本を 4 鍵に。デモの IP の許可は **AWS のセキュリティグループ sg-03676b20bd23d84ed の 443 番**(nginx ではない)に 54.199.97.180/32 を追加(sgr-0364e76ce9f663d12)。**EC2 の IAM ロール(KbDemoBedrockRole)は SG を変えられない → 利用者の saml2aws の再認証の後に手元の AWS CLI で**(poweruser-role-with-saml)。kg_api は VPC 内 `http://172.31.4.27:8600` だけ(SG で 8600 は 172.31.0.0/16 から・nginx は /v1 を中継しない)。資料 `docs/ガイド_kg_api_接続と認証_VPC内_20261030.md`。


(索引から移した説明・2026-11-07)
- [API 鍵の交換(決327〜330)](api-key-rotation-20261028.md) / [compose の正本と鍵の組](compose-env-canonical-kg-api-keys.md) — 段 A・B 完了・旧鍵は期限なく残す(決330)・env を替えたら .env も同じ便で・決341 で 4 つ目 f1fa83e3 追加・IP 許可は SG 443(手元 CLI で)

**決356(2026-11-14 02:02 JST・利用者の依頼と承認)**: 5 つ目の鍵 **11200c21** を追加(MCP サーバー `~/jrtokai-mcp-server` 専用・読み取りだけ・既存 4 鍵は不変・KG_API_WRITE_KEYS は EC2 に設定なし)。値は手元 `~/.config/jrtokai-mcp/kg_api.key`(0600・52 字)だけ・kb_backup.sh の S に追加。EC2 は recreate_env.py で作り直し(退避の像 pre-key5-20261114・瞬断 17.1 秒・docker diff は空だった)+ .env 3 本を 5 鍵に(EC2 の道具 ~/t327/key5_prepare.py・key5_verify.py・key5_env_canonical.py)。env_canonical_update.py は 3 鍵を assert していて使い回せない。新アカウントの複製は 4 鍵のまま(切り替えの時に足す)。手元からは `ssh -N -L 127.0.0.1:18600:172.31.4.27:8600 kb-demo-ec2`・ヘッダーは x-api-key。
