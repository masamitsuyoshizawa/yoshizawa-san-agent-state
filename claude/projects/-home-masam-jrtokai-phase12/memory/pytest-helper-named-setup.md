---
name: pytest-helper-named-setup
description: 試験のファイルの補助の関数を setup と名づけると、pytest 7 系は自動で走らせて準備の誤りに成る(8 以降では起きない)
metadata:
  type: feedback
---

試験のモジュールの、ただの補助の関数に `setup`・`teardown` の名を使わない。pytest 7.4.4 は nose 互換の機能で、モジュールの `setup` を自動で呼び(引数が在れば、モジュールを渡す)、全部の試験が準備の誤りに成る。手元の pytest 9.0.3 では起きないので、自分では気づけない。

**Why:** 2026-11-24 に、rev(pytest 7.4.4)が rkb の試験 29 件で見つけた。同じ名が test_r1_cut.py・test_r1_inputs.py(a07453e2f)にも在り、7.4.4 では 81 件が誤りに成る事を実測した。

**How to apply:** 補助の名は `mk` などにする。他の者が回す試験は、相手の版でも回す(`pip install --target <一時の置き場> pytest==7.4.4` と `PYTHONPATH` で、手元の環境を変えずに試せる)。
