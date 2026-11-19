---
name: agent-newcomer-briefing-deck-20261105
description: エージェントの新担当者向け 全体説明の資料(2026-11-05 用)= SMテンプレの pptx 10 枚 + 表紙・目次・生成は build_deck.py・色の決まり 橙/紺/緑/灰
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-11-04T13:33:14.475Z
---

2026-11-04 に利用者の依頼で作成。明日(11-05)エージェントの新担当者(SAT・暗黙知に詳しい技術者)と営業・責任者へ全体を説明する資料。

- 置き場: `docs/資料_新担当者向け全体説明_20261105/新担当者向け_全体説明_20261105.pptx`(メモあり・sha16 88936e6e50726a6e)と `…_メモなし.pptx`(相手へ渡す用・sha16 8ab28b62b081bbff)。どちらも 11-04 の版・書体はメイリオ(利用者の指定)。利用者は WSL の中のファイルを見つけにくい → Windows の名(\\wsl.localhost\Ubuntu\home\masam\jrtokai-phase12\docs\…)を添えるか SendUserFile で渡す。生成 = 同じ所の `build_deck.py`(venv の python・python-pptx でテンプレの表紙と目次を埋め、3 枚目の版面に描いてから元の 3 枚目を外す)。
- 色の決まり(全頁で同じ): 橙 = エージェント側(jr-central-agent)・紺 = 当方の API 側(jr-central・kg_api = SAT-DKE)・緑 = SAT 検索層・灰 = 外部(AWS・Azure)。
- 構成: 全体像 / 2 台の EC2 の中身 / LLM の使い方 / 機能①指令対応 ②事故摘録の横断 ③図表の問い / ユーザフィードバック / 検討①暗黙知の置き場 ②事故摘録の追加 3 か所 ③AWS 移行。
- 表紙の題は「エージェントの全体説明」(「新ご担当者向け」は外す・利用者指示)。3 頁は論理の図 = SAT 標準と SAT-DKE を並べ、エージェントが呼び分ける形(SAT 標準の検索層を実際にはエージェントの EC2 で動くことは注記で)。エージェントの作り = FastAPI + uvicorn(Python 3.12)が画面と API を返す・画面は素の JavaScript(枠組みなし)・流れ図は mermaid・処理は LangGraph・Bedrock は boto3(確定・検証用の版のコード)。
- 当方の案は載せない(利用者の回答 11-04)。検討① (d)・検討③ Q3 は選択肢と論点だけ。
- 機能の流れは検証用 7691 の版(develop b69b3a4)のコードで確かめたもの。本番 7687 は 10/14 の旧い版でコード未入手(推定と明記)。
- 確かめた事実: 評価 = 回答ごとの ○△× + 理由 5 つ + コメント 2,000 字・履歴の JSON に書き足す(別ファイルなし・外へ送らない)。生成モデル = Sonnet 5 global(既定)・図表の問いは Haiku 4.5 jp(RENDO_QDEC_QA_GEN_MODEL)・地域の既定 東京。

**Why:** 新担当者へ渡す共通の地図。後で内容を直す依頼が来うる。
**How to apply:** 直すときは build_deck.py を直して作り直し、LibreOffice で画像にして確かめる(メイリオは ~/.local/share/fonts に Windows の書体ファイルへのリンクを置いてあり、LibreOffice でも本物で描ける)。関連 [[aws-account-migration-20261031]]・[[intake-accident-sat-registration]]・[[tacit-knowledge-kickoff]]・[[tech-talk-materials-20260928]]。


(索引から移した説明・2026-11-07)
- [新担当者向け 全体説明の資料(20261105)](agent-newcomer-briefing-deck-20261105.md) — SMテンプレ pptx 10 枚・build_deck.py・色 橙=エージェント/紺=当方/緑=SAT/灰=外部
