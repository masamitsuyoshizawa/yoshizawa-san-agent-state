---
name: sat-api-sandbox-check-fed
description: SAT の公開 API の読むだけの確かめ(2026-11-13)= 手順書の接続先は本番(environment=production)・Sandbox の鍵は 401・Sandbox の接続先の受領待ち
metadata:
  type: project
---

coord req 20261113-1913(利用者の了承 = 読むだけ)。手順書と回答は accident_kb_v7/data/incoming/sat_api_20261113/(git 外・0600)、鍵は ~/.sat_api.env(sha8 fc55ae46・Sandbox の鍵と利用者の答え)。結果(rep 20261113-1928・GET 9 回・鍵つき 3 回・書く 0): 手順書の接続先の動作確認の口は 200 で environment = production / 鍵つきの仕様と一覧は 401 Invalid API key / Swagger も 401。**Sandbox の接続先は手順書にも回答にも無い**(回答は「試しは Stockmark の dev 環境」)。推し量って別の接続先は試さない。次 = SAT チームから Sandbox の接続先を鍵と組で受け取る → 読むだけの確かめを回し直す(GET だけの道具を作り直す)。

**Why:** 本番の顧客のデータの環境に誤りの鍵を試し続けない。決349・決350 の SAT 登録の保留は変えていない。
**How to apply:** 接続先・ID・ファイル名は連絡と文書に書かない。呼び出しの数は、道具の呼び出しごとに数えてから報告する(1 回目の報告で 7 回・2 回と数え違えた)。項目 4: 既定の extract_content_llm_type = gemini = 生成 AI に渡る(推定)。項目 7: 鍵は発行した人の権限(作成・削除まで)。[[sat-strategy-is-for-other-customer]] [[intake-accident-sat-registration]]

利用者向けのまとめ = Artifact https://claude.ai/artifact/3D6Zqd1anWebTuCHqybUW6(private・元の HTML はスクラッチパッド)。**利用者の指示(2026-11-13): 「鍵は書かなくてよいが、URL を書かないと確認が取れないので URL は記載する」** → 版 2 で接続先の URL を記載(手順書から機械で差し込み・鍵の値は無し)。連絡箱(git)に URL を書かない決まり(coord の依頼)は別で、利用者に渡す頁には URL を載せる。

分析セッション jrtokai-agent-env の共有(2026-11-13・資料 ~/jrtokai-agent-env/docs/15_指令長AI_SATのAPIの呼び出し_抜き出し_20261113.md・未コミット): エージェントは同じ EC2 の検索層 AA(127.0.0.1:18000)の POST /v2/query を**認証なし**で呼び、SAT の鍵の設定は無い = 公開 API(X-API-Key)とは口も認証も別。公開 API で足した dataset が AA の読む Azure AI Search の索引に入るか・parserModel/chunkMethod の一致(エージェントは sat_parser/page で絞る・外れると黙って 0 件)・手元の索引(485 件)にも同じ名前で足す要がある、は不明。鍵のファイルの形は問題なし(40 字・引用符/空白/CR/BOM なし・export なし)。

**2026-11-16 テスト環境の接続先を受領(coord・決349 追補 2)**: `~/.sat_api.env` に SAT_BASE_URL / SAT_HEALTH_URL / SAT_DOCS_URL を足した。GET だけ 22 回: 鍵なし 401「API key required」・誤った鍵 401「Invalid API key」・**Sandbox の鍵(fc55ae46)は認証を通る**。仕様は鍵なしで `/v1/openapi.json`(道 32 本・dataset の一覧の口は無い)。鍵つきの GET は、存在しない dataset も POST 専用の口も無い道も、同じ 404「Not Found」= 口の有無を GET では区別できない。**動作確認の口の environment はテスト環境でも production を返す(11-13 にこの欄を本番の根拠にしたのは誤り)。** エージェントのデモ: 同じ EC2 の検索層(127.0.0.1:18000)→ Azure AI Search を直接読む・公開 API は呼ばない・接続先の名に dev の語(指紋 c4160ba5)・エージェントの dataset の ID 2 つはテスト環境で 404。索引が本番の SAT と同じ物かは不明(SAT チームに尋ねる)。書き込みを伴う確かめ(架空の文書で作成〜削除)は利用者の了承待ち。

**2026-11-16 訂正と登録の試し(coord・決349 追補 3・4)**: (訂正) テスト環境で働く頭の道は **`/v1`**(受け取った `/rag/api/v1` は鍵が通った後どの口も 404「Not Found」)。上の段の「エージェントの dataset はテスト環境で 404」は**誤り**(道が無い時の応答を読み違えた)。正しい道では、エージェントの設定の dataset 2 つ(指紋 2d3a4729 = マニュアル 18 件・6eaf44c4 = 事故摘録 496 件〈success 484〉)が**テスト環境に在り、Sandbox の鍵で読める** = デモはテスト環境の側のデータを使う見込みが強い。(試し・利用者の承認・架空の文書だけ・dataset 5 つを作って全部削除)= 作成 → 送る(`/upload/vector-dataset/packed` へフォームで)→ 待つ → 一覧 → 検索 → 削除は通る。**dataset の状態は送った直後から available を返すので、完了はファイルごとの processing_status で見る**(1 回目はこれを取り違えた)。3 件で 5〜7 分(待ち行列が大半)。**既定の道では Excel は PDF に変換され、セルからはみ出した字が取り込まれない(success のまま)・送る時の項目 use_xls_parser=true ならセルの値がそのまま入る**。PDF と変換された Excel の本文は Gemini の説明の文。/query は 401「Invalid API key」(/retrieval は通る)。記録 = 本体ツリーの git 外 accident_kb_v7/data/incoming/sat_api_20261113/trial_20261116/。教訓: 「見つからない」の応答は、口が生きている対照(在る物への問い)を取ってから読む([[absence-claim-read-whole-path]]・[[negative-test-check-reason]] の再発)。

**2026-11-16 午後 検索層の口と公開 API の突き合わせ(coord・決349 追補 5・利用者の承認「両方・事故摘録への問いも可」)**: 検索層 AA の `POST /v2/query` は、エージェントの EC2 の中から認証なしで使えた(必須は query と parser_model・アプリと同じ本文 = retrieve_only・deterministic・sat_parser・targets に dataset_id / channel vector / provider azure_ai_search / index_name / inline_search_profile.chunk_method page・working_set.top_k)。dataset の ID と索引の名は EC2 の `~/sat-jrc-agent-staging/environments/staging/.env` の RENDO_VECTOR_DATASET_ID・RENDO_INCIDENT_DATASET_ID・RENDO_VECTOR_INDEX_NAME から、その場で読む(値は出さない)。同じ 4 問を検索層 3 回・公開 `/v1/retrieval` 2 回: **公開の 20 件のうち 18 件が検索層の上位 10 件と同じ(ファイル, 頁)で、18 件とも本文の指紋が同じ = 同じ索引を読む(強い推定)**。ただし並べ方は 4 問のうち 1 問ではっきり違う(どちらも繰り返しでは不変・理由は不明)→ **公開 API でデモの検索の品質を代わりに測れない**。検索層は検索だけでも 1 問 約 1,900 トークンの生成 AI を呼ぶ(retrieval_helpers)・応答 134〜438 KB。デモの事故摘録は Excel(xlsx・xls)で、タグの組(table 2・description 1・content 1)が既定の道(PDF 変換)と同じ = 字が切れる道で入っている見込み(実際に切れているかは未確認・原本との機械の照らしが次の案)。並べ直し(reranker)と回答つきは試していない(枠を減らさないため)。報告メモ `docs/報告_SAT_APIの確かめと登録の試し_coord_20261116.md` §4-7(main ca312de88 の版)。

**2026-11-16 午後 デモの事故摘録の本文と原本の Excel の照らし(coord・決349 追補 6・利用者の承認)**: デモの事故摘録 496 件のうち 352 件が、登録済みの Excel の原本(acc:0349〜0700)と名で一致(残りは xls 131 ほか・手元に原本なし)。公開 `/retrieval` に一般の語の 40 問 → 原本の在る 88 頁(81 事故)を、セル(10 字以上)と本文の `<content>` の 4 字の並びで照らした: 全部入っている 87.3%・一部が違う 9.9%・**頭だけ(切れの形)12 セル = 0.4%**・見当たらない 2.1%・字では約 97.4%。**架空の Excel で見た大きな切れは、実物ではほとんど起きていない**(実物は結合した広いセル・1 シート 1 頁)。本文は生成 AI の書き写しで、一字も違わないセルは 86.2%・数の並びは 2,392 のうち 1 だけ無い。**隠しのシートも 1 枚 1 頁で索引に入っている**(352 件の全部で 頁の数 = 全シートの数・隠し 717 枚は中身 4 種類だけ = 様式の共通のシートと見られる・中身は未確認)。公開 API に頁の画像の口 `GET /v1/evidence/download`(PNG)と、まとめての取り出しの口 `GET /v1/datasets/{id}/download`(未使用)が在る。検索層はファイル名で絞れる(`target_extra_filters` = {dataset_id: {or_filters: [[{field: metadata.fileName, op: eq, value}]], fail_on_filter_error: true}})。**事故摘録の文を問いとして送らない**(SAT へ送らない決まりに当たる)。記録は trial_20261116/ の xlsx_check_*(数だけ)。報告メモ §4-8(main a386aba4e の版)。
**How to apply(追記):** 案に書いた範囲を越える呼び出し(今回は頁の画像 2 回と検索層 4 回)をしたら、報告の中で必ず名指しで知らせる。

**閉じた(2026-11-16)**: coord がテスト環境の接続先を受けて確かめと試しを済ませた(決349 追補 2〜7・docs/報告_SAT_APIの確かめと登録の試し_coord_20261116.md)。fed の残りの作業なし・req 20261113-1913 を closed にした。

**2026-11-18 試し直し(coord・決352 追補 93)**: 配る版の PDF(実装 8ff57d454 で作った架空の 3 頁・sha16 155f9330066895cf)を、テスト環境に既定の設定で登録 → success・表のタグと説明の文 0 = 字の層のまま入る。ハイフン・半角の空白・補助の面の字は、手元の読みと同じ数。行の頭の全角の空白だけ落ちる。待ち 325 秒。作った dataset は一覧を確かめてから削除(404)。**この試しは、公開の口を手元から呼ぶだけで、エージェントの EC2 へのログインは要らない**(coord が利用者に何度も頼んだのは誤り。EC2 が要るのは、EC2 の中の検索層 /v2/query と比べる時だけ)。道具はスクラッチパッド sat/trial_retry.py(再起動で消える)。


(索引から移した説明・2026-11-20)
- [SAT の API の確かめ(20261113〜16)](sat-api-sandbox-check-fed.md) — テスト環境の頭の道は /v1・架空の文書で登録の試し済み・Excel は既定で字が切れる・検索層 /v2/query と公開 /retrieval は同じ本文だが並べ方は違うことがある・今のデモの事故摘録は字の切れ 0.4% で少ないが隠しのシートも頁として索引に在る(決349 追補 2〜6・報告メモ)
