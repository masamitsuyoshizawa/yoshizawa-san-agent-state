---
name: hyperv-port-memo-location
description: CLAUDE.md §7 が指す Hyper-V ポート予約の記録の実体は phase12 側の memory。2026-10-29 時点で動的範囲は既定のまま(1024〜)で、9658〜10257 を避けても再起動で別の帯が予約されうる
metadata:
  node_type: memory
  type: reference
  originSessionId: 85f25ae1-06b2-4f77-8b2b-5e740c5782d5
  modified: 2026-10-28T17:07:17.671Z
---

CLAUDE.md §7 の `~/.claude/.../memory/windows-hyperv-port-exclusion-blocks-docker-publish.md` の実体は `/home/masam/.claude/projects/-home-masam-jrtokai-phase12/memory/` にある(mvp2 の memory ではない)。

2026-10-29 02:0x に kgb が読み取りで確認: 除外範囲は 9658〜10257 に無く(起動ごとに移る)、動的範囲は開始 1024・数 64511 のまま。mvp2 の Neo4j 案はポート 9390/9377(演習 9391/9378)・127.0.0.1 のみ(`docs/設計_Neo4j専用コンテナ_kgb_20261029.md`)。

**How to apply:** コンテナが Up なのに接続できないときは、docker を疑う前に `/mnt/c/Windows/System32/netsh.exe int ipv4 show excludedportrange protocol=tcp` で帯を照らす。恒久策(管理の除外・動的範囲の変更)は利用者の判断で、勝手に行わない。
