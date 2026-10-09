---
name: rev-session-codex
description: rev セッション(OpenAI Codex・セカンドオピニオン専任)を 2026-09-14 に新設。worktree ../jrtokai-phase12-rev(ブランチ rev)・AGENTS.md・契機 3 つ・要回答不従属・較正試験 2 件で導入可否
metadata: 
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-09-11T09:46:17.926Z
---

2026-09-14 利用者指示で rev セッション(OpenAI Codex CLI 0.147・WSL)を coord 配下に新設。
- 役割: セカンドオピニオンのみ(共有コードの EC2 反映前レビュー・承認前の方策案批評・監査結果の独立確認・決定論計算の再導出)。実装・評価・グラフ書き込み・EC2 操作・判定器代替はしない。指摘は「要回答・不従属」(採否は当事者+coord・理由を rep に)。
- 環境: worktree `/home/masam/jrtokai-phase12-rev`(ブランチ `rev`・git 管理外データを含まない)。指示書は `AGENTS.md`(Codex は CLAUDE.md でなく AGENTS.md を読む)。立上げ手順 `docs/revセッション_立上げ手順_20260914.md`。Codex は SendMessage を受けられない=連絡は docs/comms のファイルのみ・起動は利用者。
- 較正試験: req 20260914-1100(app.py judge 区画の独立レビュー・既知欠陥 "\n\n" 2 バイト差を見つけるか)・1105(監査体系の独立確認)。2 件で有意な指摘 0 なら導入しない。試行 2 週間。
- **実績(2026-09-18)**: 顧客向けプレゼン p14〜p17 のレビューで**数値の母集団誤り 5 件を提示前に検出**。(a) 11%→83.5% を矢印で結んでいたが母集団も測定単位も別物(11%=不正解 321 件の分析・正解原因文の設備語が D-KG 語彙に存在した割合/83.5%=確定原因文 508 件中 424 文に辞書の設備表記が含まれる文単位) (b) 帯別 5 区分の合計は 563(判定対象)で 601 事故の内訳ではない (c) 194 件は束ね(DUPLICATE_OF・削除しない)で除外件数でない (d) 帯別報告は初見帯にも正答があるので「無い障害は当たらない」「精度の天井」は不成立 (e) p15 下端の描画切れ。全採用。派生して coord が CSV を点検し「すべての関係に出典を保持」も不成立と判明(出典・過去事象への紐づけは 6,909 本中 3,114 本)。
- **自動連絡の仕組み(2026-09-19)**: `scripts/rev_dispatch.py`(new/resume/status/verify)+ `scripts/rev_panes.sh`(tmux 3 ペイン)。`codex exec` と `codex exec resume <ID>` で**非対話のまま文脈を継続できる**(常駐 TUI は不要)。**オプションはサブコマンドの前**(`codex exec --sandbox … resume <ID> <PROMPT>`)。**Codex のサンドボックスは .git への書き込みを拒むため rev は自分でコミットできない**(exit 128)=ステージまでで、コミットは coord が代行する。起動 ID は `--json` の構造化イベントからのみ取る(本文を正規表現で走査すると、レビュー資料中の過去 ID を拾って誤配送する)。**自動起動は見送り**(dec 20260919-0800)。受入条件は件数でなく重大な失敗経路の遮断(5 条件)。当面は利用者が `!` で手動起動。
- 注意: `~/.codex/config.toml` は本体ツリー(個人情報データあり)を trusted 登録済 → rev は本体ツリーで起動しない。追跡中 `kb_demo_v3/docker/neo4j_v3.env` に非既定パスワードあり(rev ツリーに含まれる・利用者へ報告済)。

**Why:** 同一モデル同士のレビューは盲点を共有しやすい(fed が見つけた "\n\n" 差のような別視点の指摘を得る)。**顧客提示物では特に有効**: 自分で作った資料の数値は、元報告の母集団まで遡って検算されない。
**How to apply:** rev への依頼は coord が req を置き、利用者が `! python3 scripts/rev_dispatch.py new <req>` で起動(dec 20260919-0800)。req は冒頭に (a) 今回の差分 (b) 対象版 (c) 判断してほしい一点 (d) 期限の有無 を置く(rev 指摘)。**対外資料は提示前に必ず rev へ回す**。指摘を受けたら rev の引用に頼らず coord 自身が元報告・生成コード・実データで再確認してから採否を決める。rev ブランチ取り込み時の競合自動解消は docs/comms/*.md に限る。関連 [[session-comms-protocol]] [[session-comms-worktree]]。
