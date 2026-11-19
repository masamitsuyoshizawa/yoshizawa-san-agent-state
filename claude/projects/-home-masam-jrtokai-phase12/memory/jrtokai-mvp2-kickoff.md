---
name: jrtokai-mvp2-kickoff
description: 新タスク jrtokai-mvp2(JR東海 車両関係の図表・文章の構造化・グラフ化・2026-10-29 開始)。本体 ~/jrtokai-mvp2(main・data/ git 外)・制御 ~/jrtokai-mvp2-coord(coord)。汎用性は求めず車両専用・検討記録を dynfw へ転用
metadata:
  type: project
---

2026-10-29 01:1x 利用者の指示で立ち上げ(phase12 の coord が準備)。本体ツリー `~/jrtokai-mvp2`(main・初回 0c5849d54・git 外データ `data/`)・制御セッションの worktree `~/jrtokai-mvp2-coord`(ブランチ coord)。GitHub の origin は未設定。

- 狙い = 車両関係専用の図表読取・構造化・グラフ化(汎用性は求めない)。ただし検討過程を `docs/03_ノウハウ転用の記録様式` の様式で `docs/検討記録/` に残し、`~/diagram-dynfw-phase0` のサブセット様式 v1 へ一般化して転用する(片方向・利用者経由・漏洩点検 G/M/C)。
- 置き場 = `data/intake/`(利用者が置く受け箱・読み取り専用化)→ `scripts/intake_manifest.py` で目録(`docs/目録/`・sha16・名は目録だけ)→ `data/raw/<種類>/<sha16>.<ext>`(444)。`data/private/`(700)に鍵・名簿・leak_terms。
- サブセッション = 図表の種類 × 役割(読取・構造化・検証・審査・調査)× 道具(Claude Code / Codex / Antigravity)。目録の後に `docs/02` の A/B/C から決める(coord の仮の推し = C 混合 = phase12 の ckb/dkb/rev/agy の形)。
- 参考 = `~/jrtokai-traincar/internalMeetingMemo20261027.txt`(cp932・社内 WG 10/27)。要点は `docs/00` §4(個人名は書かない)。
- 環境 = 当面 `~/jrtokai-phase12/venv` を流用・Neo4j 専用コンテナは未作成(ポートは Hyper-V 予約 9658〜10257 を避ける)。

**How to apply:** 新しい mvp2 セッションを立てるときは `docs/04_起動プロンプト_各セッション_<日付>.md` §1 の文を貼る。phase12 の規約(連絡箱・絶対規則・sha の版併記・heredoc で連絡文を書かない)はそのまま継承。関連 [[diagram-dynfw-phase0-kickoff]] [[agent-env-repo-kickoff]]

**引き継ぎ(2026-10-29 03:0x)**: 利用者が mvp2 の各セッションを起動(Claude 側の名 = jrtokai-mvp2-coord-af・dgr-round1-drawing-plan・dcr-document-reading-plan・kgb-schema-neo4j-plan・revise-acc-masking-design〔acc〕。rev=Codex・agy=Antigravity は一覧に出ない)。phase12 coord は立ち上げの内容を mvp2 coord に SendMessage で引き継ぎ、以後 ~/jrtokai-mvp2 は mvp2 coord の所有(phase12 coord は依頼が無ければ触らない)。受け箱 16 ファイル(drawing 3・manual 1・maint_standard 1・work_standard 8・case_report 2・plan_bg 1)・決1・第 0 巡 req ×6 まで済み。

(索引から移した説明・2026-11-07)
- [jrtokai-mvp2 立ち上げ(車両 図表・文章)](jrtokai-mvp2-kickoff.md) — ~/jrtokai-mvp2(main・data/ git 外)+ ~/jrtokai-mvp2-coord・車両専用・検討記録を dynfw へ転用・目録→種類→セッション構成(2026-10-29)
