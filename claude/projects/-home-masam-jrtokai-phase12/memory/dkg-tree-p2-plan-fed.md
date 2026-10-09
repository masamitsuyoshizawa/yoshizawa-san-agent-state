---
name: dkg-tree-p2-plan-fed
description: 探索木 決165 P2(fed)計画書 8f72496a 提出済み・/v1/dkg/tree・Mermaid 生成規則・Dify は securityLevel loose・Q 番号を揃える・承認待ち P2-1〜6
metadata:
  node_type: memory
  type: project
  originSessionId: 45cb04dc-7649-4ced-a8ee-420be939b12c
  modified: 2026-10-01T14:30:49.630Z
---

**I2 実装済み(2026-10-02・rep 20261002-0154・コミット d3866e05/414e8d2b)**: dkg_tree.py 998e4abec776979a・router 加法・プラグイン 0.3.11 f9056ed8(with_tree 既定 false・false は v0.3.10 と byte 同一)・計画 改訂 5 397f1ef7(参照表示「Qn 質問(本文は一覧)」の 1 通り・§17 実装の決定)。E7 全一致・E10 warm p95 1.59 秒/準備 10 秒/同時 4 で最大 5.29 秒・E3 11.16.0 strict で実データ 30/30・**E11(2026-10-02・81 回・0.857 USD・rep 20261002-0209)= true だけの失敗 2 件**: 図の参照表示の文「Q2 質問(本文は一覧)」で答えると状態整理の LLM がそれを原文として観測に入れる/11 手の長い対話で JSON を返さず文を書き写す → P2-4 を止めた → **決171 で案 A 承認**: 状態整理のプロンプトに 2 行(探索木・資料参照は原文ではない・JSON だけ/測定 id は ms:/ns: をそのまま)・DSL guided 4c56c79e7efb60bf・diagnose 5bd5ec36597bdd50(コミット 25ba8a1c)・dify_plan_a に宣言。やり直し(rep 20261002-0241・本件 2.78 USD): 参照表示と ns: は直った・**長い対話は LLM 出力では直らず**(記憶の窓 20 で 11 手目に event が落ちる既存の制約)・E11 道具に本番の入力整形(code 節点)を通した実効の状態を追加 → 実効の観測は期待どおり(戻った手は「反映できなかった」の注意)。→ **決172: E11 合格(本番の流れで読む)・P2-4 承認** → DSL 2 本に with_tree=true(guided 92a7021066e7e972・diagnose a15608916f3e12f9・コミット a83a49f0・dify_plan_a に決172 の段)。**EC2 反映ではプラグイン 0.3.11 を先に入れ、識別子を読んで KB_NEW を替え DSL を作り直してから取り込む**(DSL の依存は 0.3.10 のまま)。EC2 反映は bkb 実走と rev I4 の後に coord がまとめて諮る。既存の弱点: ns: の測定 id を false で ms:0001 と取り違え・測定も asked に入る。dkb の tree_state は計算しなかった枝を branches に入れない → 枝は answers 基準で回す。実データの見出しは大半が参照表示(長い・—→ を含む)。 **決169(2026-10-02)で第 1 段の実装を承認・P2-8 = 資料参照。改訂 4 = sha16 5c4f95f985bb0a19(コミット ec16300c・rep 20261002-0124・I0)**: C168-2 = 見出しは通常・guided とも同じ 1 行「■ 次に確認すべきこと(既存診断の参考の候補・…)」・P2-8 の題名の規則(命令/安全語/欠落/出典のみ → 資料n)・meta.safety_ledger(+rows_invalid_by_reason)・I1 の呼び方 = dkg_tree_state.py の tree_state(event, observations, asked, top, branch, mode, budget_s) -> dkg-tree-state/1(diagnose 同梱・呼び直さない)。次 = coord の照合 → I2 実装(LOCKS・OWNERS 登録は coord)。 **改訂 3 = sha16 00220f0040ab99f7(コミット 424e6984・rep 20261002-0034)**: R167-1・3〜6 と決168。stops = {code, scope root|item|branch, at, on_value, reason}(dkb と同形)。P2-7 = rev 案(true のみ: 収束宣言と見出しを中立化・Q 行の下に参考の印・番号外の手順/初動/方法本文は資料参照・通信失敗は区分判定不能)。**状態整理の LLM の目印「次に確認すべきこと」と Q 行の書式は変えない(プロンプト変更は別承認)**。新 P2-8 = 隔離の形(推し 資料参照)。 **改訂 2 = sha16 05cb01db44bcb11b(コミット 6579c943・P1 改訂 2 fb5d8ae4 に名と値を合わせただけ: basis_mix は dkb の allowed_only/outside_only/mixed/none・消失は枝単位(11.3%)・予算 5 秒・cold 13.1 秒は起動時の準備で吸収)。** **改訂 1 = sha16 2c83481e87bbf57c(コミット 6de5073d・rep 20261002-0002)**: rev R165-1・3〜7 と決167 を反映(承認 P2-1/2/3/6・P2-5 条件つき・P2-4 DSL true 化は E11 独立台本と費用の後・新 P2-7 = Dify の先頭の注意)。枝は preview_allowed だけで決める・value(正規形)と label を分ける・不明は core に渡さない・見出しは削らず参照表示・dkb が欄の名を全部採った(normalize_observation・tree_state が一次判定・qg:)。

2026-10-01 に fed が決165 P2 の計画書を出した: `docs/計画_探索木_口とMermaidとDify_fed_20261001.md` 改訂 0(sha16 8f72496a7d0a1bbe・コミット 9a1f20c4)。rep 20261001-2329(needs_user_approval: yes)。対の dkb P1 = 改訂 1 d8259fa31327c235(`dkg-state/1`)。決166 で P1 の B1〜B6 は判断済み(未確認の節点も見通しを描き「区分未確認 実施前に確認」の停止枠・A3/A4 は描かない・再計算 7 回)。

- 口 `POST /v1/dkg/tree`: 要求は diagnose と同じ **`event`**(req の `question` から変えた)・depth は 1 だけ(他は 422)・内部の diagnose の結果を同梱・fed は組み立てと描画(新 `kg_api/sources/dkg_tree.py`)。
- **EC2 の Dify 1.16.1 は mermaid 11.16.0・`securityLevel: "strict"`・htmlLabels true**(S0・coord rep 2333)。手元の 2.0.0-beta.2 は 11.4.1・loose(計画 改訂 0 はこちらを書いていた → info 2334 で予告・rev の後の改訂 1 で直す)。生成側の符号化は 2 重の守りとして残す。11.16.0 strict で例の図は描けた(LR 1,046×846)。
- 図は `flowchart LR`(TD は横 2,120 画素で小さい)・節点 12・`mermaid@11.4.1` を npm から作業場に入れ完全版 chromium で描画を確認済み。
- **状態整理の LLM は「Q1 はい」を「次に確認すべきこと」の番号で原文を拾う** → 木の質問は一覧の中だけ・Q 番号を揃える。dkb の list_no は質問/測定で別々なので fed が q_no を通し番号で作る。
- プラグイン `with_tree` 既定 false(false は byte 同一)・SVG は第 1 段に入れない・A2 は描かない案(= dkb B7)。
- 印なし(未確認)は質問 1,162/1,266・測定 1,633/1,716(name・standard・method)。

**Why:** 次の手番(rev P3 → 利用者承認 → 実装)で、計画の前提と未確認点(EC2 の Dify 設定・E11)を取り違えないため。
**How to apply:** 実装に入る前に S0 の結果と P2-1〜6 の承認(dec)を確かめる。関連 [[playwright-pdf-needs-full-chromium]] [[fault-tree-stage3-fed]]
