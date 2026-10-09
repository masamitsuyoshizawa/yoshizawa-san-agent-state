---
name: silent-path-drift-masks-cause
description: git 外データを他セッションの worktree の絶対パスで参照すると静かに壊れ、握りつぶしのせいで別の理由の NG に化ける
metadata: 
  node_type: memory
  type: feedback
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-19T01:22:12.954Z
---

git 外データの参照先が移動しても、`if os.path.exists(...)` で囲ってあると**例外が出ずに先へ進む**。
その結果、本当の理由(コーパスが読めない)ではなく**別の理由の NG**(「chunk_id が索引にない」44 件)が出る。

2026-09-19、`ingest_propagation.py` の `CORPUS_RF` が `jrtokai-phase12-rev/a_corpus/RF.jsonl` を
指したままで、coord が a_corpus を本体ツリーへ**移動**したあと追随していなかった。演習は G12 で落ちたが、
NG の文言は真因を指しておらず、私は最初「索引の行除去で出典が消えた」と読みかけ、
「44 件の出典はいかなる索引でも裏づけられない」と書きかけて撤回した。実際は出典は健在で、
パスを正すと NG 0 件。同型の欠陥が `a_map_join.py` にもう 1 件あり、そちらは未検知だった。

**Why:** worktree はブランチ切替で中身が変わり、所有も別なので、他セッションのツリーにある git 外データは
いつ移動されてもおかしくない。握りつぶしがあると、壊れたことが**別の症状**として表に出るので、
原因を取り違えたまま「データが失われた」という重い誤った結論へ進みやすい。

**How to apply:**
- git 外データは**本体ツリー**の絶対パスで参照する(他セッションの worktree を指さない)。
- 読む前に ARTIFACTS 登録の sha を照合し、**無い・違うなら専用の例外で中止**する。握りつぶさない
  (この型は `t0_extract_snapshot.py` の C4 に既にある。新しい流儀を作らず、そこへ揃える)。
- **前提の欠落**と**検査の NG** を別の例外にする。前提の欠落は再試行で直らないので、
  「--resume で再試行可」と出してはいけない。
- 試験は「落ちること」だけでなく**理由の文言**まで assert し、**是正を戻す変異**で実際に FAIL させて確かめる。
- 門に**登録されていない試験は走らない**。`TESTS` 相当の一覧に入っているかを、投入経路ごとに確かめる
  (`test_propagation_schema.py` は dec の実施条件だったのに、G0→G12 で 1 度も走っていなかった)。
- 同じ誤りが他にもないか `grep` で一斉に洗う。他セッション所有のものは触らず、所有者へ回す判断を仰ぐ。

関連: [[derived-edge-drift]] [[negative-test-check-reason]] [[verify-what-the-target-actually-reads]]
[[ingest-effect-unmeasured]] [[assumed-current-state-without-checking]]

- 2026-10-09(決218 R の前後比べ): 前の版を `git worktree add --detach` の写しで回したら、変えない手番 24 で本文が違って見えた。写しに git 外データが無く、木の口が返す**質問そのもの**が違っていた(判定の差ではない)。**版の前後比べは同じツリーのまま、変えたファイルだけを差し替えて回す**(`git show <rev>:<path> > old.py` を読み込む口 `--tree-src` を検査に足した)。
