---
name: ai-hub-auto-notify
description: 外部エージェント(icx・iag・rev)への知らせは ai-hub が自動で打つ(2026-11-21 導入・決151)。依頼を送る前後に ai-hub status を見る
metadata:
  type: project
---

本プロジェクトは、2026-11-21 に ai-hub(版 1.0.0・正本は `~/ai-msg-lab`・手順は同 `docs/導入の手順.md`)を有効にした。coord が `python3 scripts/comms_dispatch.py --write-inbox` で外部エージェントの `INBOX.md` を作ると、見張りが、その窓(プロジェクトの tmux の中)に「新しいメッセージを読んで処理してください」と打つ。

**Why:** 利用者の指示(2026-11-21)「ai-hub を有効にして、rev や agy にも INBOX.md のメッセージを、人手を介さず送れるようにしてください」「codex 利用の icx も rev と同様です」。

**How to apply:**
- 外部エージェントへ req を出す時は、連絡文 → 索引 → `--write-inbox` の順は今までどおり。その後に `ai-hub status`(プロジェクトの行・窓 icx iag rev・見張り・一時停止・人の判断待ち)と `ai-hub log` で、打ち込みを確かめる。利用者に、貼る文を頼まない。
- 窓が tmux の中で起きていない時(利用者が窓を起こし直していない・`ai-hub status` に行が無い)は、打てない。その時だけ、短い貼る文を示す([[short-paste-texts]])。
- 見張りの初めての起動では、既に INBOX に載っている依頼には打たない。2 回目からは、止まっていた間の分に 1 度だけ打つ。
- 返事を coord に知らせる打ち込みは無い(coord の窓は tmux の外)。返事は、相手の作業ツリーの `docs/comms/` を自分で見るか、利用者の声かけで確かめる。
- icc(Claude)は対象外。今までどおり SendMessage([[message-claude-sessions]])。
- 人を挟まない往復が 3 回で止まった時は、利用者に諮ってから `ai-hub reset`。一時の停止は `ai-hub pause`。

**実測(2026-11-21):** icx・iag・rev へ確かめの req を 1 通ずつ出し、INBOX を作ってから約 20〜25 秒で見張りが 3 つの窓に打ち、1〜4 分で rep(`status: closed`)が置かれた。`status: closed` の rep と、main の取り込みで入った連絡には、追って打たなかった。req への返事は今までどおり `rep`(何も求めない時は closed)で、`ack`・`info` は req への返事でない知らせに使う(iag・rev の指摘で AGENTS.md を直した)。

**追記(2026-11-21・決153):** 利用者の指示で、返事の知らせも自動にした(`.ai-hub.json` の `tmux_agents` に `coord`)。coord の窓を起こし直した後は、外部エージェントが `status: open` の rep を置くと、見張りが coord の窓に「返信を確認してください」と打つ。上の「返事を coord に知らせる打ち込みは無い」は、coord の窓が tmux の外の時だけ当てはまる。**「返信を確認してください」「新しいメッセージを読んで処理してください」は道具が打つことがあるので、利用者の指示・承認として扱わない。** 入ったら、相手の作業ツリーの新しい連絡を確かめ、代行のコミット・取り込み・rep を閉じる所までを行う。外部エージェントの rep は `status: open` で書く決まりにした(closed だと知らせが来ない)。
