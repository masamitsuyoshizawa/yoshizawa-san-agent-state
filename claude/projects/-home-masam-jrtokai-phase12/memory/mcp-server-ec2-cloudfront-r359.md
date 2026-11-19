---
name: mcp-server-ec2-cloudfront-r359
description: 決359(2026-11-14): 読み取りだけの MCP サーバーを EC2 kb-demo-ec2 に置き、専用の CloudFront(E33JTQCG5ZIX8K)と専用の WAF で公開した・構成と戻し方
metadata:
  type: project
---

決359(2026-11-14・利用者の承認)。`~/jrtokai-mcp-server`(git の管理下でない・道具 9 本・口 8610)を EC2 kb-demo-ec2 に配備。旧アカウント(末尾 4458)。

- EC2: `~/mcp/`(ソースの束 sha16 3b8e87919647087e・像 jrtokai-kg-mcp:0.1.0-20261114・`secret/` に mcp.env と鍵)。コンテナ jrtokai-kg-mcp = **172.31.4.27:8610**(nginx はコンテナなので 127.0.0.1 では届かない)。
- nginx: vhost `mcp.<IP>.nip.io` を足した(設定 sha16 c1b72aa5e4fe6e14・退避 `~/nginx_backup/nginx.conf.pre-dec359-20261114`)。**CloudFront が付ける秘密のヘッダー X-Origin-Verify が合わなければ 403**。証明書は jrtokai-demo-acme(acme.sh・webroot)で `docker exec` で発行 → `/certs/mcp/`。
- AWS: 配信 E33JTQCG5ZIX8K(d2n371tyrytr1z.cloudfront.net・CachingDisabled・AllViewerExceptHostHeader・待ち 90 秒)・WAF jrtokai-mcp-ip-allowlist + 集まり jrtokai-mcp-allowed-ips(us-east-1)・SG の 443 に pl-58a04531(sgr-0a1e00c8b93ef9dbf)。
- 手元 `~/.config/jrtokai-mcp/`: mcp_client_cf.key(下流の鍵 sha8 520733b1)・mcp_cloudfront_url.txt・origin_header.secret(6fc73f42)・waf_allow_ips.txt(許可の IP・値は文書に書かない)。
- 原本の口は `/meta` の masked が真でなければ遮断。伏せた版は決352 段 6(決351 の後)。

**Why:** 顧客の Kiro から kg_api を読むため。既存の CloudFront E3GPIYOTTCREU6 はエージェントの EC2 の物で、本 PJ の EC2 の前には配信が無かった。
**How to apply:** IP を足す時は waf_allow_ips.txt → `aws wafv2 update-ip-set`(lock token 要)。EC2 を止めて起こすと公開の番地が変わり、MCP 用の名・証明書・配信の元を直す。WAF の説明の欄に括弧は使えない。関連 [[api-key-rotation-20261028]] [[ec2-nginx-conf-canonical]] [[aws-account-migration-20261031]]
