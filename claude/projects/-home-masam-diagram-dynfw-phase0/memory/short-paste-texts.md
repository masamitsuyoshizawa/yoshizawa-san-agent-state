---
name: short-paste-texts
description: 他セッションへの「貼る文」は短文だけ。2026-11-21 から、外部エージェントへは ai-hub が自動で知らせるので、貼る文は ai-hub が止まっている時だけ示す
metadata:
  node_type: memory
  type: feedback
  originSessionId: fdc4c948-da33-4847-b9a6-04212d37500d
  modified: 2026-10-23T20:34:14.897Z
---

各セッション(icc・icx・iag・rev)に利用者が貼る文は、原則として「INBOX.md の新しい依頼を処理してください」程度の短文で示す。依頼の中身・守ること・止まり方は req 本文に書き、貼る文で重ねて繰り返さない。

**Why:** 利用者が 2026-10-24 に「単に新しい依頼を実行して、と言うだけではだめか」と尋ねた。AGENTS.md が作業開始時に INBOX.md を見ると定め、INBOX には `git merge main` の手順と要対応の req 名が載るので、短文で足りる。

**How to apply:** 貼る文に中身を書かない。順番の指定・着手の条件・req の後に変わった事情・続きの促しも、全て新しい連絡文(req か info)に書き、索引と INBOX を作り直してから、短文だけを示す。

**追記(2026-11-18):** 利用者が「iag への依頼は『新しい依頼を処理してください』とだけ記載し、内容は規定の通信手段に準拠するようにしたい」と指示した。coord が、続きを促す中身を貼る文に書いて示したのを正された。前に書いていた「例外のときだけ一言足す」は取り消す。

**追記(2026-11-21・決151):** 利用者の指示で ai-hub を有効にした(`.ai-hub.json`)。外部エージェント(icx・iag・rev)へは、coord が `comms_dispatch.py --write-inbox` で INBOX を作ると、見張りが窓に知らせの文を打つ。**利用者に、貼る文を頼まない。** 貼る文を示すのは、ai-hub が止まっている時(`ai-hub status` にプロジェクトの行が無い・一時停止・人の判断待ち・`enabled` が false)だけにする。返事を coord に知らせる打ち込みは無いので、返事は、利用者の声かけか、coord が相手の作業ツリーを見て確かめる。詳しくは [[ai-hub-auto-notify]]。

関連: [[delegate-mechanical-checks-to-ai]]
