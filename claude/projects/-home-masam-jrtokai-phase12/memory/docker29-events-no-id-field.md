---
name: docker29-events-no-id-field
description: "docker 29.x の events の書式に古い欄 {{.ID}} が無く rc 64 で外れる・{{.Actor.ID}} を使う(28.5.1 でも使える)・dind の起こし直しは /run を tmpfs に"
metadata:
  node_type: memory
  type: reference
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-23T03:32:04.509Z
---

docker 29.4.1(EC2 の版)の `docker events --format '{{.Action}} {{.ID}}'` は「can't evaluate field ID in type *events.Message」で rc 64 になる。古い欄(ID・Status・From)が外れたため。`{{.Actor.ID}}` は 28.5.1 と 29.4.1 の両方で完全な ID を返す。`docker ps --format '{{.ID}}'` と `docker images --format '{{.ID}}'` は別の書式で、29 でも使える。

2026-11-23 に、coord の段の道具の決着の口(settle_helper)がこれで、正常な未発行も unknown と判じていた(手元 28.5.1 の試験では見えず、29.4.1 の dind で見つけた・b9 f38faf914 で直した)。

dind(docker:29.4.1-dind)で daemon の起こし直しを確かめる時は `--tmpfs /run` を付ける。付けないと、容器を起こし直した後に dockerd が API を開かなかった(古い pid の残りと推定)。

**How to apply:** EC2 で使う docker の書式は、手元の版ではなく EC2 と同じ版の Engine で当ててから固める。[[test-fixture-must-match-prod-setup]]
