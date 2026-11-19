---
name: copied-tool-hardcodes-overwrote-other-repo
description: 別リポジトリへ写した comms_dispatch.py が phase12 のセッション一覧と作業ツリーの場所(../jrtokai-phase12-<略号>/INBOX.md)を決め打ちしており、mvp2 で回して phase12 の INBOX 7 つを上書きした(2026-10-29 01:46)。道具を写すときはディレクトリ名・略号・話題の決め打ちを先に grep する
metadata:
  type: feedback
---

2026-10-29 01:46:57、新リポジトリ ~/jrtokai-mvp2 に phase12 の scripts/comms_*.py を写して `comms_dispatch.py --write-inbox` を mvp2 の作業ツリーで回したところ、SESSIONS = phase12 の 7 略号・出力先 = `../jrtokai-phase12-<略号>/INBOX.md` が決め打ちで、phase12 の INBOX をすべて mvp2 の内容で上書きした(rev の INBOX に mvp2 の req が載った)。dynfw も同じ道具を写していたが、そちらは .gitignore に scripts を入れていた(回していない)。

**Why:** 道具は書いた PJ の構造を前提にしている。写して動いたように見えても、書き込み先が別 PJ の作業ツリーだった。「INBOX.md を書いた: fed(...)」の出力に phase12 の略号が出ていたのに、新 PJ の略号が出るはずという期待で読み飛ばした。

**How to apply:** 他 PJ の道具を写したら、回す前に `grep -nE "<元 PJ 名>|略号の列挙|TOPICS|expanduser|os.path.join\(parent" scripts/*.py` で決め打ちを洗い、REPO/SESSIONS/TOPICS/BRANCHES を先に直す(mvp2 は 2bf900f27・以降で是正)。書き込みを伴う道具の初回は、書き込み先のパスを表示だけさせて確かめる。事故が起きたら、元 PJ の coord ツリーで元の道具を回して復元し、info で「上書きの時刻・復元の時刻・その間は INBOX を信用しない」を知らせる。関連 [[jrtokai-mvp2-kickoff]] [[silent-path-drift-masks-cause]]
