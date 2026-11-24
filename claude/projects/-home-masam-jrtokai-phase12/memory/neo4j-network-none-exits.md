---
name: neo4j-network-none-exits
description: neo4j:2026.03.1-community の使い捨ての容器を docker run --network none で起こすと、起動の直後に止まる(終わりの値 3)
metadata:
  type: reference
---

2026-11-24 12:3x(手元の docker 28.5.1): `docker run -d --network none -e NEO4J_AUTH=… neo4j:2026.03.1-community` は、記録に「Neo4j Server shutdown initiated by request」と出て、すぐに終わりの値 3 で止まった(誤りの行は出ない)。網の指定を外す(既定の bridge・口は公開しない)と 6 秒で答えた。原因の細部は不明(調べていない)。

**How to apply:** 使い捨ての Neo4j を外から切り離したい時は、`--network none` でなく、口を公開しない既定の網で起こす。待ちの間に答えが無い時は、まず `docker ps -a` で終わっていないかを見る。消す時は rm -v([[disposable-neo4j-rm-without-v-leaves-data]])。
