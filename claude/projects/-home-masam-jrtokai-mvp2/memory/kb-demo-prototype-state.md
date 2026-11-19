---
name: kb-demo-prototype-state
description: coord のデモ scripts/demo/kb_demo.py(決42)は kgb の build_plan と check_input._synthetic に依る・127.0.0.1:8741 を LOCKS に予約中(利用者が止めるまで)・説明とスライドの Artifact の URL(2026-11-02)
metadata:
  node_type: memory
  type: project
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-01T16:46:33.871Z
---

2026-11-02 に coord が作った知識ベースの問いのデモと、説明の公開物の状態。

- **デモ**: `scripts/demo/kb_demo.py`(0b1e328・selftest 20/20)。
  - git 外の kgb_input 15 本を、`scripts/kg/ingest.py` の `build_plan`(DB に接続しない)でメモリの上のグラフにする。
  - 図面の切り出しは、`scripts/drawing/common.py` の `load_masked` の門を通す。
  - LLM なし・Neo4j なし・ファイルに書かない。
- **ポート**: 127.0.0.1:8741 を LOCKS に予約中(保持は coord・期限は利用者が止めるまで)。止めたら LOCKS の行を解放する。dgr の採否の画面は別の番号を使う。
- **自己試験の依り先**: selftest は `check_input._synthetic` を読む。kgb が `scripts/kg` を直したら、取り込みのときにデモの selftest を回し直す(kgb の注記・info 0144 の既読)。図面の kgb_input は 2026-11-04 に段 B で書き出し直された(rep 0214・cfab14d・XRef と RevisionMark を足した)。デモはこの版を読める(決44 §2)。ただし KB は起動の時にだけ読むので、今の画面は起動の時の版のまま。新しい版を見せるには起動し直す(info 0144 の「開き直せば」は誤りで、決44 §4 で訂正した)。画面には XRef と RevisionMark を出す欄がまだ無い。
- **公開物**(非公開の既定・一般化した内容だけ):
  - 説明のページ https://claude.ai/artifact/9xVQf89TGBZFGMxX1Ad3x1
  - 元のファイル(説明のページ・スライド)は `docs/work_coord/説明資料_20261102/`(613a540)。直すときはここを直し、スライドは root を `slides_deck` にして出し直す
  - スライド 19 枚 https://claude.ai/artifact/FC6rK7Jb7ZqEe5Kjnf7jam
  - 直すときも、公開の前に `leak_check.py files` を通す(決42 §3)。
- **受入の独立**: 決42 で、利用者がデモで b08e と文書 12 本の値を見うる。acc は、凍結の info に、見たのが記入の前か後かを書く。

関係: [[shape-check-prints-keys]]・[[acceptance-independence-no-element-kinds]]

**解析の状況のタブと段の台帳(2026-11-04 22:47・c53b534・3f10d1c)**: デモの最初の画面を「解析の状況」(15 本 × 段)にした。
- 秘匿・入力の検査・演習・正本・受入の期待・照合の段は、git の中の台帳 `scripts/demo/progress.json` に書く。状態と根拠の連絡文の名だけで、値と件数は書かない。読取の段は kgb_input の status から画面が出す。
- **取り込みの手順に足すこと**: 段が進んだ連絡(凍結の info・正本への投入・演習・書き出し直し)を取り込んだら、台帳の該当の段と `updated` と `banner` を直し、selftest(実の台帳に check_ledger を回す)を通してからコミットする。kgb_input が書き出し直されたら、デモを起動し直す(起動の時にだけ読む)。
- 今の起動は背景のタスク blsf1stk5(main の木・8bbfaba・2026-11-05 02:38)。台帳は起動の時にだけ読むので、台帳を直したら起動し直す。止めたら LOCKS の行を解放する。
