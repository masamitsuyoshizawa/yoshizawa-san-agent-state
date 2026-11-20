---
name: tmux-34-pitfalls
description: tmux 3.4 を道具から操る時の罠 4 つ(hook でサーバーごと消える・問い合わせが rc 0 で空・=名前 の対象・名の頭の一致)と、pty の試験の罠
metadata:
  node_type: memory
  type: feedback
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-11-20T06:21:06.326Z
---

tmux 3.4 を、道具(ai-hub)から操った時に、実測で分かった事(2026-11-20・`~/ai-msg-lab` の版 1.0 の実装中)。

1. **`client-detached` の hook で `kill-window` を呼ぶ作りは、使わない。** 窓が先に消えて(中の過程が終わって)、その窓だけを映していたセッションが消えた後に hook が走ると、**tmux のサーバーごと消えた**(全部のセッションが無くなる)。
2. **問い合わせ(`list-windows` など)が、成功(rc 0)なのに、空で返る事が在る。** 映している端末(クライアント)が消える瞬間に、240 回に 1 回ほど。空のはずの無い問い合わせは、少し待って問い直す。「窓が 0 だから、セッションを消す」の判定は、一覧が読めた(1 つ以上)時だけにする。
3. **`set-option`・`set-hook` の `-t` は、`=名前` では「no such session」。`=名前:` が要る。** `has-session`・`kill-session` は `=名前` で通る。`destroy-unattached on` を、まだ誰も付いていないセッションに設定すると、その場で消える。
4. **窓を名で指すと、名が無い時に、名の頭が一致する別の窓に当たる。** 止める時は、番号(`@N`・`#{window_id}`)で指す。
5. **pty の試験**: 読み手のスレッドが `os.read(master)` で待っている間は、別の所で `os.close(master)` しても、端末は実際には閉じない(次の出力まで、数秒遅れた)。読み手は `select` で待ち、止めてから閉じる。

**Why:** 1 は、本番なら、同じプロジェクトのエージェントの窓が全部止まる所だった。2 と 5 は、通しの試験が「たまに落ちる」原因で、5 を知らずに、道具の側を疑って時間を使った。

**How to apply:** tmux の側の後始末は、hook でなく、呼び手の後始末(SIGHUP を受けて finally)と、見張りの片付けの 2 段にする。間の長さを測る試験は、壁の時計を使わない([[wsl2-wall-clock-steps-back-every-30s]])。変異の試験は、落ちた理由まで照らす([[negative-test-check-reason]])。関連 [[ai-msg-lab-auto-trigger]]。
