---
name: gate-allowlist-not-denylist
description: 正本へ繋がせない門は、正本の host:port を拒む一覧(文字列の一致)でなく、台が自分で立てた接続先だけを許す一覧にする
metadata:
  type: feedback
---

接続先の門を「正本の host:port の一覧に一致したら拒む」で作ると、同じ口に届く別の書き方(`0.0.0.0`・`[::1]`・`host.docker.internal`・コンテナの名・scheme の違い)が抜ける。門は「台が自分で立てた一時の DB の URI(書き方まで同じ)だけを許す」にし、Neo4j の形の URI と口を明示した URI は全部この一覧で照らす。書き込みの置き場も「台の中だけ」を許す形にする。

**Why:** 2026-10-31 の決343 D3 で、dkb の台の commit 版の門は kb_conn の正本の一覧の文字列の一致で照らしていた。coord が別のプロセスで、この 3 つの書き方が正本 B の 9890 に TCP で届くのに門を通ることを示し、要是正になった(決343 追補 1)。否定例も、拒む一覧と同じ一覧から作っていたので、穴を落とせなかった。

**How to apply:** 門の否定例は、門が使う一覧から作らない。届く口の全部 × host の書き方 × scheme の違い、を外から並べる。変数の名で決める規則は、利用者の環境の変数(`DBUS_SESSION_BUS_ADDRESS` など)に当たるので、値の形で決める。製品の `kb_conn.is_canonical` にも同じ穴があり、coord が窓口で別に扱う。関連: [[tightening-is-not-safety]] / [[negative-test-check-reason]] / [[next-phase-challenges-ab]]

追記(2026-10-31 10:30・決344 の受入): coord が製品の kb_conn を「手元の host は口の番号で照らす」に直した版でも、正本 B の 9890 に TCP で届くのに拒まない書き方が 9 つ残った。`127.1`・`0`・`2130706433`・`0x7f000001`(ipaddress.ip_address は受けないが getaddrinfo は受ける)・`localhost.`・`ip6-localhost`・`ip6-loopback`・`kubernetes.docker.internal`・この機械の FQDN。数え上げで拒む形は、どこまで足しても抜ける。否定例にはこの 9 つと、届くかどうかの TCP の確かめ(繋いで直ちに閉じる)を入れる。dkb の台の門は 59/59 で止める(rep 20261031-1029・推しは演習モードも許す一覧に)。
