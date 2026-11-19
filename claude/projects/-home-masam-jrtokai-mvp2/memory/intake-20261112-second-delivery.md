---
name: intake-20261112-second-delivery
description: 2026-11-12 の追加の受領(zip 158 ファイル・新しい 134)と顧客側の予備検討の資料(QA 203 問)の置き場と扱い・全体の目標(構造化の試行・phase12 と同じ API とエージェントの構成)
metadata:
  node_type: memory
  type: project
  originSessionId: 2112c51c-7795-468b-b1eb-6be18b1f709b
  modified: 2026-11-12T04:11:54.040Z
---

2026-11-12、利用者が検証用のデータを追加で届けた。目録は `docs/目録/目録_20261112.md`(公開版・名なし)。

- 受け箱: `~/jrtokai-mvp2/data/intake_20261112/`(読み取り専用)。zip を展開した物は `data/staging_20261112/`。名を含む目録は `data/private/目録_full_20261112.md` と `data/private/intake_20261112_manifest_full.json`(種類・raw の置き場・個人情報の型の当たりを持つ)。
- 新しい 134 ファイルを `data/raw/` に写した: drawing 2(174 頁)・manual 5(1807 頁)・case_report 6・work_standard 108・regulation 13(新しい置き場)。検証中の 15 ファイルは同じ sha で含まれていた。
- 図面と説明書の PDF は、スキャンの画像に OCR の文字層が付いた形。Word と Excel の 99 ファイルは LibreOffice で PDF に変換できた(`data/derived/intake_20261112_conv/pdf/`・崩れは未確認)。
- 個人情報はどれも未確認。図面と写真は、マスクと人の確かめまで開かない。
- 顧客側の予備検討の資料 3 つ(xlsx 2・pptx 1)は raw に写さず、`data/private/qa/` に置いた(sha16 の名)。問い 203・期待する回答 130。

**Why:** 利用者の当面の全体目標は、各資料の構造化を試行して実現性と課題を確かめること。構成は jrtokai-phase12 と同じく、API と、API を使うエージェントの形を考える。

**How to apply:**
- 決78(2026-11-12・`docs/07` 第 1 版): QA は基本は開発用に使ってよい(取り分けは取り下げ)。代わりに監査の決まり Q1〜Q9(phase12 の X1〜X16 と同等)で、API とエージェントが答えを記憶しない仕組みを徹底する。問いと回答の文を、リポジトリ・連絡文・docs・メモリに写さない。後日届く QA は、作り手に見せる前に acc が 1 回測る。
- 決78 のほかの判断: エージェントははじめ Claude の道具呼び出しで作り、できるだけ薄くする・資料に無い物は無いと明示して返す(顧客に追加を頼まない)・API とエージェントは kgb・Neo4j は 1 つのまま・Codex と Antigravity も十分に使う(git の中の物の審査と調査は CLI で直接頼んでよいと coord が受け取った)。
- 図面の人手の確認は、全部の図面に同じ手間を掛けられない。大幅な自動化と効率のよい確認の方法が要る。ただし、読取の課題とノウハウを得るため、今の 3 枚の作業(2a8c → b08e)は続ける(利用者の判断)。
- 漏れの検査: 原本名の語片 192 を `leak_terms.txt` に足した。8 字の一覧は 88 万に増えた(誤検出が増える)。Word と Excel の原文は 8 字の検査に入っていない。
- coord の持ち分: 監査の手順書と機械の検査の道具(Q1〜Q4)・監査の台帳・漏れの検査を Word と Excel の原文に広げる直し・rev と agy の起動のプロンプト。各セッションは自分の画面の Plan Mode で計画を諮ってから始める。

関連: [[acceptance-independence-no-element-kinds]]・[[leak-check-banner-false-positive]]
