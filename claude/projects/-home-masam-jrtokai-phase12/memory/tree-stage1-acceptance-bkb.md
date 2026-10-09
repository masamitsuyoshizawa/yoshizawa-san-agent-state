---
name: tree-stage1-acceptance-bkb
description: 探索木 第 1 段(決169)の bkb 受入設計の状態・未決 Q1〜Q9・期待の作り方の要点
metadata:
  type: project
---

2026-10-02 決169 I3: 受入設計 docs/設計_受入_探索木第1段_bkb_20261002.md 改訂 1(0cfbc5b86b7f97bc・コミット 551be0b8)。基準 P1 改訂 4 d6dd184f826c4548・P2 改訂 4 5c4f95f985bb0a19。受T1〜T8。

- 葉の期待は**変更前のコミットの旧口 diagnose を top=10000** で(`cands[:top]` は出力を切るだけ。受T2-0 で実物で確かめる)。新しい `_diagnose_core`・`tree_state`・`dkg_tree.py` は開かない。
- next_questions の各項目は question・kind・measurement・gain・disc を持つ → 項目の選び方も独立に再現できる。
- LOO 647 = knowledge_kb_v8/data/eval/dkg_loo700_cases.jsonl(8e862e690fe9324f)。
- 未決: Q4 閉。Q2/Q3/Q6/Q7 一部。Q1(参照表示の文言 3 通り)・Q5(行 digest の正規形)・Q8(依頼の文と P1 §11-4 の違い)開。**Q9 = 台帳/manifest が無いと全ての木が root で止まる**(開)。開が残る項目は実走しない。
- 2026-10-02 実走済み(決171・rep 20261002-0433): 合格 T1/T3/T6/T7/T8・F1(描く順が一覧の順・fed)・F2(no_dec の理由・dkb)・F3(target_changed が meta に出ない・dkb)・F4(点数1位の範囲と字数の定義なし・fed)・否定例 9/9。証跡 knowledge_kb_v8/data/eval/bkb/tree_s1/run_20261002-0251。
- 台の教訓: (1) git archive は git 外のデータ(kb/data・index)を落とす → 丸ごと写して変わった追跡ファイルだけ戻す (2) top は出力を切るだけでなく計算を変える → 基準の写しに旗つきの差し込み(_bkb_all)で全候補 (3) 候補は中身でなく cid で(B 追補は同じ cid で文が変わる)。
- 道具: tree_s1_acceptance.py・run_tree_s1_acceptance.py(段 prep/t1/tree/exp/t4/t6/judge)・judge_tree_s1.py・check_tree_s1_negatives.py。

**Why:** 同じ誤りを両側で使う照合では検出できない(rev R165-1・R167-5)。
**How to apply:** 実走の req が来たら §9 の状態を最新の計画で読み直してから道具を書く。関連 [[facets-instance-entry-acceptance-bkb]] [[collation-derive-expected-from-source]]
- 2026-10-02 再走(rep 0519・run_20261002-0444_rerun): F1〜F3 是正を確認・受T4 15 場合(S12〜S14 は dkb の差し込み口 DKG_TREE_TEST_HOOKS=1 + DKG_TREE_TEST_UNDETERMINABLE)合格・受T5 6,564/6,582 → **F5(fed): inspection の候補で名に EM DASH があると決まった 2 字の名(計画は候補n)**。
- **前回 F4 の 41 件の大半は bkb の期待の誤り**: 子の並びの先頭を「点数1位」とした(所見があると点数順でない)。定義 = 子の全件の score 最大・同点は子の並びで先・NFKC 後。期待は木の JSON でなく基準の全候補から作る(受T2 で同じと確認済み)。
- 2026-10-02 受T5 回し直し(rep 0553・run_20261002-0525_rerun_t5)合格 6,582/6,582・F5 是正確認。**bkb の受入 受T1〜T8 で残る不合格なし**。候補n の n = 上位の中は対応表の行番号・外は行数 + 子の並びで上位の外だけの順番(計画 改訂 7 §4-1)。
- 2026-10-02 決174(rev I4)限定再走 全部合格(rep 2125・run_20261002-2050_i4): 警告/terms 旗 ON/OFF 同一・不正 manifest 9/9 root 停止・meta の等式・重複 key・受T1。語の独立の期待 = 変更前 diagnose の safety_mark.terms({term,class,field})。複数の作業の種類は「欄「語」・欄「語」を伴う」(計画に未定義・試し回しで見てから採った=限定に明記)。
- 2026-10-03 決177 EC2 受入 全部合格(rep 0455 写し・rep 1440 独立の期待)。**ローカルで EC2 を再現するには設定を揃える**: DKB_RETURN_METHOD=1・DKB_SINGULARITIES=1・KG_FACETS_CANDIDATE_RULE=candidate-rule-v2・KG_C_URI/KG_C_PW(C の接地・kb_conn の c)・要求の top や groupings も写しの道具に合わせる(揃える前は 0 件一致)。道具 check_tree_ec2.py。
- 2026-10-05 決179 (1) b_isolation 見出しの是正(dkg_tree 8c353ff9)の受入 全部合格(rep 1636・受入設計 584671321a59baca・道具 check_tree_biso.py)。陽性対照 loo|92・147・185・321 の Q3。基準は run_20261002-2050_i4。fallback_labels の並びは描いた順(q_no 順ではない)。予測できる変化は実走の前に期待へ入れる(「図が同じなら可」に緩めない)。
- 2026-10-05 決180 第 2 段 第 1 項(受け取った回答)の受入設計 改訂 0(docs/設計_受入_探索木第2段第1項_bkb_20261005.md・f5cabc28ff1a66eb)。正本 P2 改訂 13 §18/§19-3・P1 改訂 10 §11-13。台本 W/L/C1/C2/R・入力整形 dify_plan_a.py の 5 つの return 経路・§19-3 の門は bkb が実装する。実走は dkb・fed の実装の rep の後。
- 2026-10-05 決180 第 2 段 第 1 項の受入 実走(rep 1955・証跡 knowledge_kb_v8/data/eval/bkb/tree_s2/item1_20261005-1851): HTTP は書式 G1・名 G2 を除いて合格・不変 U1〜U4 合格。**受入の台 base_env に C-KG(KG_C_URI・KG_C_PW)が無く、第 1 段の回は C の接地が全部失敗していた(0/2,654)→ 台を直した**。接地の失敗は応答に印が残らない → 回す前に grounded_equipment が空でない件数を確かめる。
- 2026-10-05 決180 第 2 段 第 1 項 限定の再走 全部合格(rep 2037・証跡 tree_s2/item1_rerun_20261005-2006・dkg_tree 11110130・tree_state b52aa51f)。期待は計画 P2 改訂 14・P1 改訂 11 の文字どおり。次は coord が EC2 反映を諮る。
- 2026-10-05 決182 第 2 段 第 2 項(質問の見出し・案 C)の受入設計 改訂 0(docs/設計_受入_探索木第2段第2項_bkb_20261005.md 7998a813・rep 2141・fed へ確認 4 つ req 2142)。bkb の門を第 1 段の回の木に当てると試作と同数(2,024 等)= 読み方の一致で合否に使わない。実走は fed の実装 rep の後・K0 で C 接続を先に確かめる。
- 2026-10-05 決182 第 2 段 第 2 項の受入 全部合格(rep 2332・固定版 fd707f25・証跡 tree_s2/item2_20261005-2149)。K1 2,709/2,709・K4 1,327/1,327。次 = 決183 甲の受入(設計 f10ca022・甲の版に固定した写しで)→ 決184 乙の受入設計(req 2243)。
