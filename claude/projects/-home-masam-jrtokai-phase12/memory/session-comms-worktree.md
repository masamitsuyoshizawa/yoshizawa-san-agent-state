---
name: session-comms-worktree
description: セッション間連絡はdocs/comms連絡箱方式・dkbはworktree ../jrtokai-phase12-dkb(ブランチdkb)で作業・EC2/正本操作はcoordへreq
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-09T06:08:17.184Z
---

2026-09-09導入(利用者承認済・fed発1335/coord発1409)。
- 連絡: docs/comms/ に YYYYMMDD-HHMM_<from>_to_<to>_<type>_<slug>.md(type=req/rep/info/dec/ack・
  YAMLヘッダ・受信側がstatus更新)。INDEXは `python3 scripts/comms_index.py` で再生成(手書き禁止)。
  禁止: 個人情報・原文断片・acc_id付き生データ(git外はパス+sha256でARTIFACTS.md登録)。
- 略号: fed(federate改善)/dkb(本セッション=D-KB改善)/coord(統合管理・../jrtokai-phase12-coord)/user/all。
- **本セッション(dkb)の作業ツリー: /home/masam/jrtokai-phase12-dkb(ブランチdkb)・2026-09-09再起動で移行完了**。mainは本体ツリー占有のため
  専用ブランチ必須・区切りでmainへmerge(ff優先)。git外データは本体ツリー絶対パス参照。
- 共有資源(B/C正本・EC2・共有スクリプト)は書き込み前にLOCKS.md予約+info予告。
  **EC2 kg_api反映・正本加法投入はcoordへreq**(dkbの暫定担当は返上済み・ack 20260909-1500)。
- OWNERS: dkg_backend.py/knowledge_kb_v8/scripts/dkg/*/run_fed_vs_dkb.py=dkb所有。
  accident_diag_engine.py/accident_client.py=共有(変更はreqで合意)。
- 利用者承認が要る件は needs_user_approval: yes でcoordが集約提示。

**Why:** 版混合事故(20260909)の再発防止と多セッション協調の正式規約。
**How to apply:** 作業開始時にINDEX.mdの自分宛openを確認。編集はdkbワークツリーで行い、mergeはcoordへreq(dkb側はff-only同期のみ)。連絡ファイル名とdateヘッダは必ずdateコマンドの実時刻(coord指摘20260909)。worktreeでの評価実行はgit外データのリンク(kg_api/kb/data・kb/index・knowledge_kb_v8/index→本体絶対パス)が前提。
