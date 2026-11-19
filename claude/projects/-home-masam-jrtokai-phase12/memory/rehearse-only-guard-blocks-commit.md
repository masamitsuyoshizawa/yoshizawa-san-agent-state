---
name: rehearse-only-guard-blocks-commit
description: 演習専用の止め(ingest_pipeline の「表の組と伏せ直しは rehearse と dry-run だけ」)が在ると、演習で通した物は正本へそのまま入れられない。「そのまま入れる」と推す前に、commit の道の止めと開始の門(台帳と 3 DB の digest)を読む
metadata:
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-11-18T01:36:10.491Z
---

2026-11-18 決351 第 1 弾: dkb は rep 1009 で「11-14 の台で通した固定版 d397393aa をそのまま正本へ・再演習は要らない」と推した。利用者がそれで承認(追補 85)した後、準備の段で次の 2 つを見つけた。
- ingest_pipeline 373 行が、非既定の表の組の unmask と rescrub の `--mode commit` を止める(「正本への口は利用者の承認の後に別の変更で開ける」)。
- commit の G0 は台帳と B・D-KG・語彙 DB の digest を照らす。接続の決めは B にしか無かった。

台は `--mode rehearse --auto-approve` なので、commit だけの道(台帳との照らし・人の承認の段・終わりの台帳)は通っていなかった。

**Why:** 演習の合格は、演習の道の合格でしかない。演習専用に作った口には、正本の側に止めが在るのが普通で、それを読まずに「そのまま入れる」と書くと、利用者の承認がその前提の上に乗ってしまう。

同じ日に 2 つ目: 工程は自分の木(ROOT/REPO)の git 外の物(台帳・統制の台帳の挙げるファイル・状態の置き場)を読み書きするので、作業ツリーからは commit を回せず、終わりの台帳も本体に届かない。

**How to apply:** 反映の段取りを推す前に、書き手の commit の分岐(`mode == "commit"` の行)と、開始の門が要る接続を、grep で全部読む。演習と commit で違う道を一覧にして、段取りに書く。どの木から回すか(その木に git 外の台帳と config が在るか・終わりにどこへ書くか)も確かめる。関連 [[tested-commit-must-be-run-commit]] [[spec-unverified-until-implemented]] [[r351-review-kit-set-dkb]]
