---
name: audit-fed-writes-fed-tree
description: audit_fed.py は報告を fed の作業ツリー(BASE)の reports/audit_fed_<日付>.md に書く — dkb から回すと fed の git 管理ファイルを上書きする
metadata:
  type: feedback
---

dkb の worktree から `knowledge_kb_v8/scripts/fed/audit_fed.py` を回すと、報告が `/home/masam/jrtokai-phase12-fed/knowledge_kb_v8/reports/audit_fed_<日付>.md`(fed の作業ツリー・git 管理)に書かれる。2026-09-23 に X1-2 の再検査で `audit_fed.py X1` を回し、fed の同日の報告を X1 の節だけに上書きした(fed に連絡し、fed が git checkout で戻した・失われた変更なし)。fed は書き先を `--report-dir` または環境変数 `AUDIT_FED_REPORT_DIR` で指定できるようにした(fed a330133・既定は従来どおり fed ツリー)。**dkb から回すときは必ず `python3 knowledge_kb_v8/scripts/fed/audit_fed.py --report-dir <dkb の scratch か作業ツリー> X1` の形で書き先を指定する**。

**Why:** スクリプトの BASE が実行元の worktree ではなく fed のツリーを指している。他セッションの共有物・作業ツリーを実行の副作用で書き換えるのは、所有の規約に反する。

**How to apply:** 他セッション所有のスクリプトを回す前に、出力先(REPORT・dst・BASE)を grep で確かめる。書き込み先が他のツリーなら、出力先を変えられるか所有者に確かめるか、コードを読んで該当の検査だけを別に回す。接続情報(KG_C_URI・KG_C_PW 等)が要ることも先に確かめる。関連: [[session-comms-worktree]]

**同じ型(2026-09-24・fed 自身の道具)**: `replay_ft0_reviews.py` は最後に「基準へ戻す」ため `gate_ft0.py` を自分で回し、観測 `probe_observations.json` を作り直す。決67 の「gate で 1 回だけ」の条件の下で、確かめずに回して 2 回目の生成を起こした(中身は同一)。**検証の道具を回す前に、それが成果物を書くか・入口を回すかを grep する**(`subprocess` / `gate` / `os.replace` / 書き出し先)。
