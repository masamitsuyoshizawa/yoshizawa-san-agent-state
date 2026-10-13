---
name: kb-excel-export
description: 4 DB(B/C/D/語彙)の Excel 書き出し(2026-10-13 顧客提出)— 道具 scripts/export/export_kb_excel.py・出力は git 外 knowledge_kb_v8/data/export/kb_excel_<日付>/・件数照合 OK
metadata:
  type: project
---

- 利用者依頼(2026-10-13): B-KB・C-KB・D-KB・語彙 DB のグラフを DB ごとに Excel にし、説明シートを付けて提出。
- 道具 `scripts/export/export_kb_excel.py [出力先] [b,c,dkg,vocab]`。接続は docker inspect の NEO4J_AUTH(ローカルの写し・読み取り専用)。`~/.kb_ec2.env` は EC2 トンネルのポート(19890 等)なのでローカルには使えない。
- 形式: 「説明」(出力日・出力元・版 digest・構成・id の読み方・DB の説明)/「ノード一覧」/「関係一覧」/ N_<ラベル>(id・labels・各プロパティ)/ R_<関係型>(start_id・start_label・end_id・end_label・各プロパティ)。総称ラベル Node はシートにしない。配列は「 | 」区切り・32,000 字で切り詰め。id の無いノード(B の Document 700・D の PropagationTerm 143)は <ラベル>#<内部 id>。
- 検証: 4 ブックとも N/R の行数が DB の件数と一致。B に電話番号の型 0 件。語彙 DB の関係型は 9 種のうち 1 種が 0 本(シート無し)。
- 注意: .data() は関係を tuple にするので properties(r) で取る。所要 約 30 秒(4 DB)。

- 2026-10-13 追加: 説明シートの末尾に列名の対訳表(`scripts/export/kb_excel_glossary.py`・鍵 → 日本語・意味・現れるシート)。B 58・C 433(日本語鍵 265 は接尾辞の規則)・D 125・語彙 46 鍵・対訳なし 0。sha16 は ARTIFACTS.md。

**How to apply:** 再出力は同じ道具で日付を変えるだけ。EC2 の DB を出すなら接続を差し替える。[[local-backup-setup]]
