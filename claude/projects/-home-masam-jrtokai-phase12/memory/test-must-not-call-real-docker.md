---
name: test-must-not-call-real-docker
description: 単体試験が本物の片付けの関数を通り、名が固定の演習のコンテナを docker rm -f で消した(2026-10-16 23:54)— 試験の中で docker を見張る
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-16T14:56:40.482Z
---

test_rehearse_human が、札(container_label)だけ偽物にして本物の remove_resources を通していた。札が「自分の識別子」と答えるので、本物の `docker rm -f neo4j-rehearse-b` が走り、裏で回していた同値性の演習の DB を復元の途中で消した(docker events で 23:54:03 の kill/destroy を確認)。

**Why:** 演習のコンテナは名が固定(neo4j-rehearse-b/-dkg)なので、別のセッションが承認待ちで保持している演習も同じように消しうる。偽物を 1 つ差し替えても、その先の本物の副作用は残る。

**How to apply:** 外の資源(docker・DB・EC2)に触れる関数を持つモジュールの試験は、setUp で副作用の関数を偽物に替えたうえ、`subprocess.run` を包んで docker を呼んだら落ちる見張りを入れる。見張りが効くことも陽性の対照で確かめる(偽物が副作用の手前で止まっていて見張りに届かない作りに注意)。重い演習を裏で回している間は、同じ資源に触れうる試験を回さない。関連: [[test-must-not-inject-what-prod-lacks]]・[[pkill-kills-own-shell]]
