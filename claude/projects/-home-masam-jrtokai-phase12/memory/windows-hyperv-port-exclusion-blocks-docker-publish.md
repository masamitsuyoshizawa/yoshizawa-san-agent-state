---
name: windows-hyperv-port-exclusion-blocks-docker-publish
description: 手元 Neo4j(9890/9990/10090/10190 等)の公開が再起動後に消える真因は Windows の Hyper-V 動的ポート予約。docker restart では戻らない。復旧は管理者 PowerShell(winnat 停止→restart→開始・恒久は動的範囲を 49152 以上へ)
metadata:
  type: project
---

2026-10-28 16:06 の PC 再起動後、手元の Neo4j 4 コンテナ(accident-v7 9890・crossing-v6 9990・dkg-v1 10090・vocab-v3 10190・各 http 9677/9777/10077/10177)と kb-v6 の bolt 9790 が、コンテナは Up なのにホストのポートが公開されない(HostConfig.PortBindings は在り・NetworkSettings.Ports は空)。

**Why:** `netsh int ipv4 show excludedportrange protocol=tcp` の Hyper-V/WSL 動的予約(100 ポート × 6 ブロック 9658〜10257)に失敗ポートが全部入り、成功ポート(9477・9577・7476・7689・15432)は全部範囲外だった(確定・読み取り)。動的範囲の設定は既定(開始 1024・数 64511)。Docker Desktop 28.5.1(WSL2)は Windows 側で bind できないとき公開だけ落としてコンテナを起こす(推定)。docker restart では戻らない(dkg-v1 で実測)。網の設定の差ではない。

**How to apply:** 「コンテナ Up なのにポート閉」を見たら、docker を疑う前に `/mnt/c/Windows/System32/netsh.exe int ipv4 show excludedportrange protocol=tcp` で範囲を照らす。復旧は利用者の管理者 PowerShell: 即時 = `net stop winnat` → coord が docker restart → `net start winnat`。恒久 = `netsh int ipv4 set dynamic tcp start=49152 num=16384`(再起動後有効)+ `netsh int ipv4 add excludedportrange protocol=tcp startport=9600 numberofports=700`(本 PJ の帯を自分で予約・ポートが空いているときに)。bkb の「本物の app」受入は healthz ok を門にする(16:07〜の回は届いていない可能性)。関連 [[env-replacement-pc-20261009]] [[pc-handoff-mechanism]]

追記 2026-10-28 18:40: `net stop winnat` は長時間応答せず、Windows 再起動で復旧(予約の場所は起動ごとに変わり、今回は 9500〜10300 に来なかった)。動的範囲は既定のままなので再発し得る。恒久の対策は利用者の判断待ち。

(索引から移した説明・2026-11-07)
- [Hyper-V 予約で手元 Neo4j の公開が消える](windows-hyperv-port-exclusion-blocks-docker-publish.md) — 再起動後に 9658〜10257 が Windows に予約され docker
