---
name: scratch-script-name-shadows-package
description: 作業用の台のファイル名を proto/alpha_contract_bridge の中のパッケージ名(diag・flow・judge・gate・confirm など)と同じにしない。台の置き場が sys.path の先頭に入り、同じ名の部品を隠す
metadata:
  node_type: memory
  type: feedback
  originSessionId: 89509479-6c46-49a0-ab87-5ecfa8b26309
  modified: 2026-11-05T04:45:26.310Z
---

2026-11-05 の req 1329 で、作業用の置き場に置いた台を `diag.py` と名付けた。そのため `pilot_cli.issue_replay` の中の `from diag import derived_flow` が台を読み、A の再生が 6 とも ImportError になった。台は例外を捕えて結果に書いたので、止まらずに気付けた。名を `g3run.py` に替えて回し直した。

**Why:** Python は、実行した台のディレクトリを sys.path の先頭に入れる。proto の部品は、関数の中で遅れて import するものがある(issue_replay など)。そのため、台の名が同じだと、途中の段だけが失敗する。

**How to apply:** proto/alpha_contract_bridge の直下のパッケージ名(`ls proto/alpha_contract_bridge`)と重ならない名を台に付ける(例: 依頼の番号を入れた名)。再生や派生の段だけが ImportError になったら、まず台の名を疑う。
