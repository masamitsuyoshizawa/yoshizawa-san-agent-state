---
name: env-replacement-pc-20261009
description: 2026-10-06 夜に開発 PC が故障し、代替 PC の WSL(Ubuntu-oldpc)で作業。手元 Neo4j は EC2 の写し・区切りごとに push・compose の一部は動かない
metadata:
  type: project
---

2026-10-09 利用者連絡: 開発 PC が 2026-10-06 夜に故障。代替 PC の WSL「Ubuntu-oldpc」= 旧 PC の 2026-10-06 22:47 時点の完全な写し(git・worktree・会話履歴・データ)。

**Why:** 手元の状態の前提が変わった。手元 Neo4j 4 つ(9890/9990/10090/10190)は 2026-10-08 に EC2 から書き出して復元したもので、旧 PC の手元 DB ではない(EC2 未反映の開発中の変更が入っていない可能性)。この PC が最新の唯一の写し。

**How to apply:**
- 手元 DB を前提に作業する前に graph_digest 等で状態を確かめる(所有者ごと)。
- 作業の区切りごとに `git push origin <略号>`(coord は coord と main)。全ブランチは 2026-10-08 に origin へ push 済み。
- docker は使えるが、PC 上のフォルダを読む compose(v3/v4 デモ・nginx)は手元で動かない。Docker Desktop は使わない。
- D: は外付け HDD。/mnt/d/jrtokai-phase12-backup へのバックアップは動く。
- 10/06 22:47 以降の会話だけの作業は失われている(coord の最後のコミットは c71e5c20 決206)。
- この PC の既定の python3 は /usr/bin/python3 で neo4j の driver が無い(2026-10-09 bkb 実測)。道具は本体ツリーの venv/bin/python3 で回す。scratchpad の控え(置き場の名前)は会話の切り替えで消えたので、置き場は連絡文や記憶に名前で残す。
- **python3(/usr/bin/python3)は neo4j を持たない**。driver 6.1.0 は `/home/masam/jrtokai-phase12/venv/bin/python3` で使える(fed が 2026-10-09 に確かめた)。道具はそちらで回す。
- 語彙 DB(10190)は旧 PC の 10/06 の正本と E1〜E3 で全項目一致(束 ec5a3ade・全件 9e36fa44・fed info 20261009-0119)。
- 2026-10-09: 利用者が saml2aws で再認証。有効なのは profile saml だけ(default は失効)。Bedrock は `AWS_PROFILE=saml`・大阪 ap-northeast-3・global.anthropic.claude-sonnet-5 で手元から届く(bkb 実測)。期限は認証のたびに変わる。

