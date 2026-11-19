---
name: unread-body-keepalive-leak
description: http.server で本文を読まずに応答すると、keep-alive の次の要求の頭に読み残しが混ざり、既定の send_error が要求の行(本文)を状態の文に写す
metadata:
  type: feedback
---

review_kit(2026-11-15)で、札の無い POST /api/save に本文を読まずに 401 を返していた。同じ接続で続く GET / の頭に読み残しの JSON が混ざり、BaseHTTPRequestHandler の既定の send_error が「Unsupported method ('{...答え...}GET')」を状態の文に写して 501 を返した(答えの値が応答に出た)。

**Why:** protocol_version = "HTTP/1.1" で keep-alive になり、読まなかった本文が次の要求として読まれる。既定の send_error は要求の行を文と本文に写す。

**How to apply:** 本文を読まずに応答する時は close_connection = True と Connection: close を付ける。send_error は上書きして、要求の行を写さない。試験は生のソケットで「POST(本文あり・札なし)+ 続けて GET」を 1 つの接続で送り、応答が 1 つで閉じることと、目印が出ないことを照らす。関連 [[review-screen-redesign-fed]]
