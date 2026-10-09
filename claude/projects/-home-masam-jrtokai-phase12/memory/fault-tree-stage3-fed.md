---
name: fault-tree-stage3-fed
description: 障害探索ツリー第 3 段(2026-09-15)の fed 分担の確定事項 — D-KG の到達構造・API/デモの形・工数・接地と名寄せの切り分け
metadata: 
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-09-15T08:16:54.146Z
---

障害探索ツリー(rev 包括案 `docs/review_rev/20260915-1210_fault-tree-design.md`)の第 3 段で、fed は探索・表示・API・性能を担当(dkb はグラフ側)。実装・投入はしていない。coord・dkb と 3 者で検算一致した確定値:

- **到達構造(D-KG 10090・完全有向)**: 症状→原因(HAS_CANDIDATE_CAUSE)→概念(NORMALIZES_TO)→検査(SUPPORTED_BY_FIELD|DETECTED_BY_FIELD)で 症状 195/1,079(18.1%)・原因 162・**概念 38**・検査 297/646。1 症状あたり検査 中央 11/p90 18/最大 73。`FieldCheck` は出辺 0 のシンクで、**検査→設備部品・測定基準は有向で 0**。
- **有向と逆行は別集合**: 有向 HAS_CANDIDATE_CAUSE 経路 195 と逆行 MANIFESTS_AS 経路 200 は共通 182・有向のみ 13・逆行のみ 18・和 213(「差の 5 は逆行で届く」は誤り=rep 1352 で訂正)。
- **測定基準まで(dkb 案1 `(概念)-[:DETECTED_BY]->(MS)`)**: 検査側から数えると検査 138・MS 29・p90 3 だが、**症状起点では症状 18・概念 9・MS 17**、検査と MS を両方出せる木は症状 11・概念 6・検査 43・MS 12。案1 は逆行ゼロで組める。EquipmentType 総数 199。
- **案2(測定基準まで・逆行あり)の症状起点は定義で大きく変わる**: 2a(=原因 c ←MS・c が概念にも正規化・逆行 1)症状 62・概念 25・MS 46、2a かつ概念→検査は症状 49・概念 18・MS 40・検査 189 / 2b(原因→概念←別原因 c2≠c1←MS・逆行 2)症状 155・概念 20・MS 43 / 概念を通らない A 症状→原因←MS は入口和 175・MS 127。coord の「175/25/46」は MATCH を 2 つに分けた式で c2=c1 を許した 2a∪2b(関係一意性は 1 MATCH 内のみ)。175 は 3 つの無関係な集合に現れた偶然(A の和とは共通 71)。**最小デモは 2a で合意**(coord 推奨・fed 同意)。
- **逆読みの規約(提案・rep 1356)**: 型ごとの読み方表を overlay 設定に持つ。区分は 2 軸 — 意味の軸(定義上=SUPPORTS_IF_ABNORMAL/CONTRADICTS_IF_NORMAL/MANIFESTS_AS、扇形=NORMALIZES_TO 逆/OUT_OF_RANGE_IMPLIES 逆)と次数の軸(全逆読みに共通の上限 初期 20)。逆読み時の分岐 最大: SUPPORTS/CONTRADICTS 8・MANIFESTS_AS 106・NORMALIZES_TO 50・OUT_OF_RANGE_IMPLIES 143(辺の本数では 220)。
- **2 段目(概念→検査)の提示順序(提案・rep 1403 §5 で訂正済)**: 2a+検査の症状あたり検査 中央 11 / p90 40(離散)・31.2(連続)/ 最大 49、MS 1/3/9。rev §6.3 に同点順キーと追加検査 3 はあるが、現データに安全区分は無く弁別数は overlay 待ち。**`DETECTED_BY_FIELD`(209)は検知ではなく除外の実績(excluded 由来)、`SUPPORTED_BY_FIELD`(475)は疑いの実績**(ingest_isolation_knowledge.py L77/87・dkg_backend.py L189)。**basis は語→概念の名寄せ方法**(det=相互包含・llm_high=LLM 高確度)で精度未測のため順序に使わない。改訂順序=安全区分→除外実績→疑い実績→id、各概念先頭 3 件、除外実績は「過去事例で除外に使われた確認」と表示し exclude へ昇格しない。逆読みで次数 20 超の終点: MANIFESTS_AS 4・NORMALIZES_TO 2・SUPPORTS|CONTRADICTS 0・OUT_OF_RANGE_IMPLIES 28(2a 本線 0)。
- **枝の爆発の主因は向き**: `OUT_OF_RANGE_IMPLIES` 2,738(MeasurementStandard→Symptom)は出次数最大 6 で密ではなく入次数最大 220。型限定+向き固定なら深さ 4 で到達最大 94、両向きだと深さ 3 で 1,984、ハブ起点 4,190。探索時間 4〜52 ms。
- **ボトルネック 2 種(別作業)**: 下側=接地(検査→設備が 0)。上側=原因の名寄せ(`Cause` 1,404 中 `NORMALIZES_TO` を持つのは 258=18%、概念 221 中 Cause から参照されるのは 54。検査に繋がるが症状から届かない 95 概念は全件孤立=入次数 0)。回復上限は検査 297→646(上限であって見込みではない)。名寄せは探索ツリーの範囲外として coord が別判断。
- **受け皿**: `EquipmentPart` 1,565 は全件 `EquipmentType` に接続、`MeasurementStandard` は `EquipmentType` に 1,709 本(部品への直接は 0)。1 部品あたり測定基準 中央 13/最大 75。
- **fed の推奨(coord 同意・利用者承認待ち)**: API は独立プロセス+読み取り専用(kg_api の BGE-M3+reranker 約 5GB と寿命を共有しない・停止だけで撤回可)、デモは独立 Streamlit(demo_v10・Dify に触れない)、着手順序は (a) 5 段デモを先に作り深掘りは接地後。fed 分担 10〜16 人日(見積)、rev の 12〜20 人日は桁妥当、LLM 費用 fed 側 0。
- 型名は B が `NORMALIZED_AS`・D-KG が `NORMALIZES_TO` で意図した使い分け(dkb)。overlay の role で吸収する方針。

**第 4 段(rev=要是正・fed 回答 rep 20260922-0600)**: 読み方表の本体は `docs/設計_読み方表_障害探索ツリー_20260922.md`、数値は `knowledge_kb_v8/scripts/fed/ft_readtable_measure.py`(D-KG digest 15935392・nodes 14,467)。
- **上の 2a の値は `via_symptom`(SUPPORTS 辺の適用条件=症状名)を見ていない上限**。起点症状と一致させると 2a 症状 31・2a+検査 症状 23(概念 18/MS 40/検査 189)・`_emb` 除外で 12/15/17/121。
- `FieldCheck` 646 は全件「B切り分け実績」(事例記録・設備欄なし)。部品名は `eq:` 接頭辞付きで、target との完全一致 25・部分一致 200(候補止まり)。`CONTRADICTS_IF_NORMAL` は SUPPORTS と同じ順向き経路から生成=独立の対偶ではない。
- min(basis_path) は廃止し注意表示 F1〜F9 を別々に集約。取得上限 20 は順逆共通・各 hop に LIMIT 上限+1。
- 最小デモは T0(dkb: 不転換→結露→ジャック板 ms:0096)・T1(軌道短絡→ボンド類 ms:1077)・T3(しゃ断桿降下不良→回路制御器→ms:0612)。全て M1 手順ノードの条件→型式内部品→HB 検査、Binding 3/3/4=10。分離は overlay 案を維持(Community 版・2,052 行は同件数の別実験 2 つ)。

**第 5 段(rev=要是正・coord req 20260916-0700・fed 回答 rep 20260922-0700・設計書改訂 1)**:
- **T0 の処置欄「—」は誤りだった**。`ms:0096`「ジャック板の確認」の方法欄と原文 HB:91:61(項目 8-2,3 の説明)は「異常時に制御リレー・回路制御器を取り外して目視」。**検査名の「確認」から観察と分類しない。方法欄と原文を必ず見る。** M1 手順 2 も「蓋を開けて」。
- 停止条件(設計書 §10): 安全区分 A0 参照/A1 外観/A2 接触・開扉(権限待ち)/A3 取り外し・分解・短絡・擬似(直前停止)/A4 取替・使用停止・押下げ(直前停止)/未確認=停止側。語の検出(MS 93/1716)は候補のみ。承認は一次 dkb/fed→二次 顧客側専門家(利用者経由)。「原文で保証された手順」は廃止。
- rev §2.4: `build_evidence_edges.py` は kind=複数経路の最小値・via=症状集合の先頭を別々に採る。現グラフで合成ずれ 2/133・複数症状 15/268。**経路単位で数えると 2a 35・2a+検査 26・exact 15**(via 一致の 31/23/12 は格納条件のもとの候補数)。探索は格納値の via/kind を使わず元の含意辺を引く。
- 原文位置: T0 M1:case3_転換不能_p04 / HB:91:61・HB:tbl:99:49、T1 M1:case2-1_軌道短絡_p04 / HB:380:181、T3 M1:case5-1_しゃ断桿降下不良_p04 / HB:253:123(ms:0612 未特定)。許可コーパスは rev ツリーの a_corpus(読み取りのみ)。
- 配置は固定 snapshot+overlay に決定(live D-KG 参照なし)。T0 先行・T1/T3 は同じ受入条件の候補。
- **分母を書く**: fed の 15/268・2/133 は支持辺の本数、coord の 24/12 は含意鎖の (MS,原因) 組 384 が分母(fed も再現)。**組 384 のうち支持辺があるのは 268・辺の無い組 116・鎖の無い辺 0** → 支持辺は現グラフより古い生成(推定・dkb 確認待ち)。
- 経路単位で増える理由は要素で確認済み: via 一致の症状集合 ⊂ 経路単位の集合、差 +3・逆 0(via は組ごとに 1 症状しか持たない)。
- **再現入口は dkb の ft_inventory.py に統一**(coord 決定)。fed スクリプトは読み方表固有の値に限り、経路単位の 2a は ft_inventory に入るまで参考値。
- 設計書 §12: 専門家確認の要否を E(必須)/R(rev・coord)/D(決定論)に分け、T0 の E は 12 項目(安全区分 5・権限者 1・Binding 3・M1 位置づけ 1・結露の定義 1・停止文言)。**E 未確認の取り外しを含む経路は対外提示しない**。

**第 6 段(rev=進めてよい・coord req 20260922-1130・fed 改訂 2 = 100762b / 6efe537)**:
- **経路単位の照合は欠落を補わない**: 経路単位の式(fed の pb・dkb ft_inventory の (iv)(v))は最初の MATCH で格納 SUPPORTS_IF_ABNORMAL を要求するので「**格納支持辺のある組に限る経路別照合**」。改訂 1 の「この古さも避ける」は誤りで訂正済み。含意鎖から直接数えても現グラフでは 2a の症状集合 35/26/15 が要素一致するが、**保証ではない**。
- 116 組の原因は空原因修復(2026-08-27)による原因ノード置換で、支持辺生成(最終 08-25)が未再実行(dkb 調査)。rev は本番修復を内部の停止表示試作の前提にしない → coord が凍結解除(5 条件)。
- §2.3 反映: 停止画面は作業の種類を省略しない短い警告+出典位置が必須。原文参照表示・手動展開は停止画面と別で、どれも停止・未承認を解かない。§2.7 反映: 予算の実測値 D と上限値の判断 R 等を分離。
- **§2.1・§2.2 も反映済み(fed 65012bb・dkb シート改訂 2 b73a8c1/c95b9dc と同定義)**: quality 4 値(verified/unverified/conflicting/unknown)+別軸 origin(reported/inferred/measured/virtual・virtual は段階 1 のみ)、observed_at/event_time/known_at 分割、observation_ref は原文 span と別。T0 の開く条件はシートの G1〜G7 の 1 式。入口 Binding(不転換→dkg:cl:3)と段 1b を追加。承認は段階 1=内部の停止表示試作(E 不要)/段階 2=意味確認済み・対外提示(Binding 3+入口 1=4 件・条件の定義・安全区分の専門確認)。専門確認は計 13 項目。req 1130 の fed 分 5 件は完了、次は coord が rev の再確認へ。

**段階 1(内部の停止表示試作)の段取り案(fed 04bbee6・`docs/段取り_T0内部停止表示試作_20260922.md`・承認前の資料)**:
- **着手は rev 第 7 段の結果と利用者承認(段階 1)の後**。coord が req 1130 §2 の「内部 T0 試作を進めてよい」を言い過ぎと訂正(bc17402)。凍結解除は「116 組の本番修復を前提にしない」だけ。
- 固定 snapshot は T0 の遷移 E0〜E7 で読む資産だけ(**dkb と揃えた版 637a170: 約 61 ノード・約 76 関係・見積**): 症状 1・型式 1・型式に当たる候補原因 15/71・概念 1・dkg:cl:3 と段 6・手順 23(手順 4 = dkg:cl:3:ph:3:st:3)・ジャック板の候補部品 3 と親 2・**MS 6(ms:0096・0068・0050・0093・0092=取り外しの条件側・0097=耐水型の気密試験)**・HB 章 1。HCW の水没管理シールは MS なし=出典参照のみ。原文 span 確定(HB:91:61 取り外し [1325,1379) 条件と行為を切り離さない・耐水型の後作業 [1383,1425)・M1 手順 4 [325,372))。原文は**コピーせず既存 knowledge_kb_v8/data/chunks を参照し検証器が sha 照合**。**支持辺・事例・機序・質問 70・故障様式・B/C は写さない**。写さない件数は manifest に残す。Checklist 系は外から入る辺が無い島。
- 完全性: 抽出時(欠けたら作らない)・読込時(合わなければ起動しない)・実行時(枝ごとに停止)。停止画面は 4 行(警告・足りないもの全部・原文の位置・段階と承認状態)。工数 約 12〜17 人日(fed 7〜10・dkb 3.75〜5.25・coord 1〜1.5)。分担確定(抽出器・allowlist・出典・overlay 中身=dkb/検証器・評価器・表示・テスト=fed)。残る判断は段階 1 の置き場(coord)。

**Why:** 利用者依頼の障害探索ツリーは第 4 段(rev 再検証)→第 5 段(利用者承認→試作)へ進む。承認後に fed が試作する際の前提値と設計判断。
**How to apply:** 試作開始時は overlay 導入後の次数を再計測する(fed が引き受けた)。数え方は rep 20260915-1333/1339/1344 の Cypher。計測の規律は [[verify-zero-counts-before-reporting]]。

## 2026-09-23 T0 段階 1 の実装着手(dec 20260923-0300 承認・rev 第 7 段反映後)
- 段取り案 docs/段取り_T0内部停止表示試作_20260922.md に rev 第 7 段 §2.1〜2.7 と §9 ファイル形式を反映。coord 決定: 原文の文(method・remarks・ChecklistStep の name)は snapshot に写さず `*_ref`({source:dkg,node_id,field,value_sha256,value_len})、他の name と standard は写す。closure の row は一意(S3-replace/S3-airtight)。
- fed 実装: knowledge_kb_v8/scripts/fed/ft0/(ft0_core=時刻の述語 T・G1〜G7 全件返し・内容 hash / ft0_validate=V1〜V5・参照欠落と未承認を別に数える / ft0_eval=E0〜E7・S2 で必ず停止・停止画面 4 行・手動展開 / ft0_fixture=架空文字列の合成 fixture / test_* 4 本)。dkb は抽出器(allowlist・C1〜C5・manifest)を担当。
- 次: 表示(独立の内部画面)・dkb の実 snapshot で検証器を通す・マイルストーン rep(ノード数・関係数・写さなかった件数)。

## 2026-09-24 rev の T0 段階 1 レビュー是正(R1〜R8・fed 9500b6b)
- rev の総括「検査の対象が入力側から与えられている」に対し、**検証の基準を ft0_contract.py(固定契約)に移した**: 14 行・leads_to(E4→S1/E7→S1b→S2)・安全定義 6・Binding 4・関係型 9 と必須関係・Checklist 有効性(STOP_SOURCE_INACTIVE 新設)・段階 1 の承認済みの組(2213 の hash)と全件 proposed・原文の置き場と許可ファイル名。構造の検証の後にだけ原文を読む。評価器も契約の鎖で二重に守る。
- **段階 1 の契約はコードに固定。経路を増やすときは仕組み自体を設計し直す**(coord)。取り直しで hash が変わったら点検合格を確かめて STAGE1_PINS を書き換える。
- rev の否定例は test_ft0_contract_negatives.py に取り込み。テスト 6 本 146 件。次: dkb の抽出器側と揃えて rev 再レビュー → 段階 1 完了報告。
- 2026-09-24 追補(fed 9e90405): dkb の突き合わせで**経路の部分集合(22 本)だけでは候補原因の関係・NORMALIZES_TO・経路外の段の欠けを検出できない**と判明し、EXPECTED_NODE_IDS 62・EXPECTED_RELS 78 本の完全一致(欠け・期待外・重複を拒否)へ替えた。E1 正準症状・b:1/b:2 の from・未接地 HCW も必須化。E2 の STOP_SOURCE_INACTIVE は overlay に宣言が無いので ROW_EVALUATOR_STOPS に分けた(取り直しで ROW_STOP_REASONS へ移す)。テスト 6 本 157 件。dkb の crosscheck(d1d715a)6/6 PASS。main 未取り込み・rev 再レビュー待ち。
- 2026-09-24 第 2 次是正(fed 15261cc・63fd178・rev 再レビュー N1〜N3): 出典束 hash(規則は dkb と一致・2213=6fbef8d3…)を STAGE1_PINS に固定・置き場の中の別名の実体を拒否・閲覧順 VIEW_ORDER を ROWS(所属集合)と分離・必須属性を 62 ノード+関係 order/rank まで読込時に拒否・対応表 2 本(fed 起点/dkb 起点)+test_ft0_trace_table.py・否定例の札は probe_ft0_reasons.py の観測で付ける。専門確認は 15 項目(検査の種別)。テスト 6 本 205+照合 9。rev の 5 巡目の再レビュー待ち(dkb の予定外し・札付け・fed 表の照合の後)。
- 2026-09-24 coord 判断(持ち越しの作業): ms:0050→S3-replace の結びつけ(推定・専門確認 #16)は段階 1 の完了条件にしない。ただし **rev の再レビュー結果を受けてから同じ便で**入れる: 契約に推定の結びつけの定数 → その資産を持つ閲覧カードに「資産の結びつけは推定(専門確認前)」1 行 → 定数を消すと注記が出ないことを検出する否定例 → 段取り案 §4.3・対応表に根拠。洗い出しの b(ms:0097 の出典範囲)・c(M1 結露→HB 8-3 錆で対象が同じか・別項目に傾く)は dkb の確認待ちで、結果を coord に共有する。
- 2026-09-24 専門確認は **17 項目**(fed 72700b0): #15 検査の種別(b は聞く 3 件/ms:0095 は dkb が是正)・#16 S3 の出典(項目 8 の注記/項目 4-2)と ms:0050・ms:0097 の関係(推定)・#17 M1「ジャック盤も確認」と HB 8-3(錆)の確認対象が同じか(違えば E7 以降が経路から外れる)。ms:0097→S3-airtight は出典で確定。#16・#17 は段階 2 の前に必須で、段階 1 の完了条件ではない。
- 2026-09-16 rev 第 3 次レビュー(T1〜T3)の是正: T1=出典束 hash の既知ベクトル(手書きの正準形)+7 欄と HB/M1 を別々に変える感度 9+対照(fed 16b28c8)。T2=否定例は CountingOpen で open と本文 read を別に数え、別名は open 0・差替えは open 1/read 0・全体 hash 不一致は read 後。札も観測で open 前/open 後本文前/本文後。変異の道具 mutate_ft0_checks.py(対照つき・19 通り)。T3=照合テストを完了ゲート化(表 2 本+要件一覧 2 本・元資料 sha256・ID 網羅・自己検査 9)、T0S 要件一覧 149 件は fed が作成(a4920ac)。dkb の段取り案起点の一覧と両表の〔要件 ID〕付け待ち。教訓: この環境の資料の括弧は ASCII「()」なので正規表現は \( \) で書く(全角と思い込むと照合器が黙って誤る・自己検査で発見)。
- 2026-09-16 req 20260924-0500(prompt-version-tie)は索引の穴で見えていなかった: federate は latest を通らず影響なし・v5.0.md が正・answer_generation の v4/v4_hybrid_addendum も同点。list_versions の修正と cypher_generation_v5.0.md の削除は利用者承認待ち(rep f80a577)。
- 2026-09-16 T3 の fed 分(fed e560fae): 段取り案起点の表に〔要件 DDR〕180 件(引用あり 138/一部のみ 23/欠け 2/範囲外 17)。照合器は〔一部のみ(未確認: ○○)〕を別集計し未確認が空なら FAIL、連絡ファイルは「元資料(本文)」で front matter を除いた hash。T0S 一覧は 152 件(dkb レビューで 3 件追加→dkb の表で覆う待ち)。dec 20260916-0500: T0S-006 と T0S-002/020 は試験を足した(変異つき)、T0S-120 は既存試験で覆う、T0S-067 は画面に check_type が無いので「欠け」。5 巡目は dkb の覆いと DDR 置き先のレビューの後。
- 2026-09-16 プロンプト(dec 20260916-0430): cypher_generation_v5.0.md 3 か所を削除(2795e72・差は規則 9 の 8 行だけと difflib で確定)、list_versions を書式と同点の例外に修正(f5b8e51・kg_api と kb_demo_v5/v6 の同一の写しを揃えた・test_prompt_manager_versions.py)。EC2 反映は coord(kb_demo_v6 は streamlit-v7 にも COPY)。prompt_store.verify(ckb)は fed レビュー合格。
- 2026-09-16 札と観測の突き合わせをゲート化(fed 9f42e5b・coord 指摘: 手で書いた「観測と札が違う 5 件」は導出されず 6 件目で黙って通る)。probe が probe_observations.json(全否定例 206 件+対象ファイル 11 本の sha256)を書き、照合器が行ごとに札と突き合わせる。裏付けの無い札は〔観測なし(意図: ○○)〕必須・一致する札への印は FAIL・コードやテストが変われば「観測が古い」で FAIL(probe を回し直す)。意図の文に ASCII の括弧を入れると印の終わりを誤読する。残る FAIL は dkb の表の印 12 件と T0S-150〜152。
- 2026-09-16 dkb の DDR 置き先レビュー(rep 20260924-1400)を全採用(fed 22559dd・6b90d43・a8efd60): 置き先違い 3 件を移動、一部のみを行に分けて付け直し(23→41)、DDR-037 に dkb の新試験。照合器に「引用のある行の最後の列に状態の欠け」を FAIL にする検査を足したが、最初は「欠け 6 に対応」(番号の参照)を誤検出→状態の書き方だけに絞り対照を自己検査に入れた。照合 PASS 64 / FAIL 0、変異 21/21。fed の分は揃い、rev 5 巡目は coord 待ち。
- 2026-09-16 EC2 のプロンプト版ずれ(coord info 20260916-0910): EC2 の kg_api が kb_demo 版の cypher_generation/v5.0.md(移行前の「route_symbol は 〇A」)で動いていた。coord が C 正本グラフの実測(route_symbol は文字飾りタグ形)で kg_api 正本を正と確定し EC2 を是正。fed の確認: streamlit-v7 は WORKDIR /app/kb_demo_v6 で kb_demo_v6/prompts を読み、NL→Cypher 画面の既定が v5.0 なので移行前の記述が実際に LLM へ渡る(kb_demo_v5 は未配備)。kb_demo 2 本を kg_api 正本へ揃える提案は利用者承認待ち。写しの同一性は常設試験 kg_api/kb/scripts/test_prompts_sync.py で守る(fed 4c9d144・いまは内容の差 1 件で FAIL)。名指しで読む answer_generation/v4_hybrid_addendum は BY_NAME に明示(版の書式検査で落とさない)。
- 2026-09-16 rev 第 5 次 V1・V2 の是正(fed c1e8dd4): V1=観測の依存集合を「置き場の *.py を全部拾い理由つき NOT_IN_DEPS で除く」向きに逆転(13 本。probe と照合器自身を含む。除外は mutate・app・iso で理由つき)。生成側と受入側で dep_files() を共用。V2=試験群ごとに rc/done/expected(fed の 4 群は所定数の宣言が無いので null+理由)/failed の見出し/completed を status に記録し、1 群でも落ちれば生成器は非 0・それでも成果物は書く(古い成功を残さない)。照合器は all_ok・status の有無・groups の鍵・expected と done を FAIL 条件に。観測 206 件は不変、照合器 PASS 64→72。教訓: 「glob('ft0_*.py')」のように名前で拾う導出は、守りたい範囲より狭くなる(入れ忘れを捕まえる向きにする)。
- 2026-09-16 V2 の穴(coord が再現): 検査を 1 件消して probe を回し直すと done が減るだけで all_ok は true のまま通った(fed の 4 群に所定数の宣言が無く、照合器は expected が数のときしか done と比べないため。U1 と同じ型)。是正: 4 群に EXPECTED_CHECKS(34・19・33・120)を置き、終了判定を「失敗 0 かつ 実施 == 所定」に。probe は宣言値を読み、null は宣言の無い群だけ(理由つき)。3 段(群・probe・受入)とも落ちることを写しで確認。教訓: 「読めない群は null と理由」のような逃げ道は、宣言が無い側に全部倒れる。
