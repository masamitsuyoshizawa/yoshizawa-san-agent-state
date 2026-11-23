---
name: docker-inspect-mounts-order-unstable
description: docker inspect の .Mounts の並びの順は呼ぶたびに変わり得る = 同じ容器でも、並びのまま指紋を取ると一致しない
metadata:
  type: reference
---

docker 28.5.1(手元・2026-11-24)で、マウント 3 つの容器に `docker container inspect --format '{{range .Mounts}}…{{end}}'` を 2 回呼ぶと、並びの順が違う事があった(決365 1-1 の台本 改訂 5 の初めの版で、同じ容器を新旧として読む試験が bash の回で外れ、dash の回は通った = たまたま)。`.Mounts` は daemon の中の対応表から作られる並び(推定)。Go の template の range は、対応表(Networks・PortBindings)なら鍵の順で安定するが、並び(Mounts)はそのまま。

**How to apply:** 新旧の一致を見るなら、マウントは項目ごとに JSON の字にしてから並べ替えて比べる(決365 の台本は、タブで区切って遠隔の記憶の中で並べ替えてから指紋)。同じ物を 2 度読んで一致する事を、くり返しで試験する(1 回の合格を信じない)。関連: [[read-tool-two-rules-state-and-enumerable-plaintext]]
