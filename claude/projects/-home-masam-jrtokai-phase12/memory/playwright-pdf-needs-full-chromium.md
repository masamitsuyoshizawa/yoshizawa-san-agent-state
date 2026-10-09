---
name: playwright-pdf-needs-full-chromium
description: "Playwright の既定 headless shell は PDF を新しいタブで表示しない(URL が \":\")・PDF の表示を確かめるときは channel=\"chromium\" の完全版で"
metadata:
  node_type: memory
  type: reference
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-30T09:51:33.720Z
---

2026-09-30(決154 R5・console の原本の「開く」)の実測。

- `p.chromium.launch()`(既定 = headless shell)で blob の PDF を新しいタブで開くと、タブの URL が `:`・`document.contentType` が `text/html` になり、表示されない。**実装の不具合と取り違えやすい。**
- `p.chromium.launch(channel="chromium")`(完全版・新しい headless)では同じ操作で URL = blob・`application/pdf`・描画も画像で確認できた。ローカルに chromium-1223 が在る(`~/.cache/ms-playwright`)。
- Firefox・Safari はこの方法では確かめていない。報告では「未確認」と書く。
- 同時に知ったこと: Streamlit 1.56 で `st.components.v1.html` に廃止予告(2026-06-01 以降)。HTML の文字列は `st.iframe(html, height=...)` で同じく srcdoc に入る(sandbox に allow-popups-to-escape-sandbox あり)。

関連: [[report-from-the-consumer-side]]
