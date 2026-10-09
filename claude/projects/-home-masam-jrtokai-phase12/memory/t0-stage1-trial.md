---
name: t0-stage1-trial
description: T0 段階 1(内部の停止表示試作)の dkb 分担の状態 — snapshot 2213・rev 第 3 次の U1/U2/T2/T3 の dkb 分は是正済み(2026-09-24)・fed の DDR 札付けと rev 5 巡目待ち
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-15T16:37:04.374Z
---

2026-09-23(連絡の日付。マシン時計は 2026-09-15)時点。dec 20260923-0300 で着手承認、段取り案は fed `docs/段取り_T0内部停止表示試作_20260922.md`、確認シートは dkb `docs/確認_T0_転てつ機の不転換から別資料の検査まで_20260916.md`(改訂 3 追補 5)。

- dkb 分担: 抽出器 `knowledge_kb_v8/scripts/dkg/t0_extract_snapshot.py`(D-KG 読み取りのみ・C1〜C5)と `t0_build_overlay.py`(自己点検)。現行は本体ツリー `knowledge_kb_v8/data/t0/t0-20260915-2213/`(62 ノード・78 関係・closure 14 行・安全区分 6 件・外観確認 S1b を E7 と S2 の間に追加・snapshot hash 7d04637f87e3bc0a・overlay_hash 7e2e5389ed799991・全件 proposed・coord 点検 94 件合格)。旧版 2148(prov_ref 写し漏れ)・2156(条件側 MS の取り違え: 「これら」= ms:0092〜0095 で ms:0068 は範囲外)は削除済み。止まる位置の独立確認は `t0_check_stops.py`(fed b9304d3 で PASS)。
- 決定: 原文の文(MS の method・ChecklistStep の remarks と name)は写さず `<field>_ref`(sha256・字数)。standard とその他の name は写し個人情報の点検対象。**点検は coord の担当(dec 20260923-1800・機械検査+全件目視)で、現行 2213 は 94 件で合格**(2148・2156 の点検は取り直しで無効になった)。snapshot / overlay を取り直したら再点検が要る。allowlist に属性を足すときは点検対象かどうかも決める。a_corpus の正式な置き場は `knowledge_kb_v8/data/a_corpus`(coord 登録)。
- **rev の段階 1 レビューは要是正(R1〜R5 すべて高・2026-09-24 req 0200)。段階 1 は未完了**。dkb 分(抽出器の R3=関係を期待構造 9 型 78 本と完全照合・R4=Checklist status「有効」必須)は是正済み(f23eeb3・実データの内容 hash は 2213 と同じで取り直し不要)。読込側 R3 と R1・R2・R5 は fed。是正後に rev の再レビュー。教訓: 検査は入力の自己整合でなく独立の仕様(期待構造)と照合する。
- **rev 第 2 次レビュー(2026-09-24 req 1100)も要是正**: N1(高)=出典の対応表が固定されていない(manifest が自分で選んだ対応を自分の hash で照合)・N2(中)=root 内の別名実体・N3(中)=手動展開が閉包表の行順を経路順に使い S1 の先の E5〜E7 が欠落。dkb 分: N1 の出典束 hash の正準化を fed と揃える(**fed と一字一句同じ定義で確定**: source_refs 7 キー ref_id 昇順+corpus_files_sha256・corpus_root 含めず・2213 の値 6fbef8d3… は両者独立に一致・抽出器の source_bundle_hash は ec76de4)/ N3 は T0 シート §7.2 に経路順の表を追加 / crosscheck は既定で厳格(契約なし・未実施は FAIL) / HAS_STEP.order など抽出側だけが見る必須属性は読込時にも拒否を推奨(fed と決める) / 対応表は 2 ファイル: dkb 作・fed 照合 `docs/対応表_T0段階1_T0シート起点_20260924.md`(ec76de4・見出し 181 件と定数 39 件を機械照合済み・予定 5 定数と欠け 8 件を明記)/ fed 作・dkb 照合 `docs/対応表_T0段階1_段取り案起点_20260924.md`(fed 作成待ち)。HAS_STEP.order などは読込時にも拒否で合意。教訓: 独立の 2 定義の一致だけでは同じ誤読を検出できない。
- **rev 第 3 次レビュー(req 20260916-0100・T1〜T3 と §8 U1・U2)**: dkb 分は是正済み(2026-09-24)。U1=ext の N1 検査を合成束の常時 14 件にし「実施 n / 所定 55」で判定・実 2213 の固定値一致は `check_t0_n1_fixed.py`(略記 n1fix・manifest 無しは FAIL・--allow-missing で NOT RUN 終了 3)/ U2=札〔除外して継続〕を追加 / T2=fed の札〔読込前拒否〕は open 前・open 後本文前・本文後の 3 区分に分割(dkb の表は fed 16b28c8 の probe を dkb で回して付け替え)/ T3=要件一覧 2 本を表と逆の作り手で作る: dkb 作 `docs/要件一覧_T0段階1_段取り案起点_20260916.md`(DDR-001〜180)・fed 作 T0S-001〜149。dkb の表に〔要件 T0S〕を置き 149 件を覆った(引用あり 127・欠け 1・範囲外 21)。fed の照合器 `test_ft0_trace_table.py` が完了ゲート(元資料 sha256・ID の包含)。**照合器の癖**: 一覧の中の表の行はすべて ID 行として読む・行に引用があれば「引用あり」(意味は見ない)・「欠け」の語がどこかにあれば欠け扱い。是正後に rev の 5 巡目。「是正した」と「main から見える」は別(コミットまでを 1 まとまりに)。
- マイルストーン ②(fed)・③(dkb)は報告済み。保留(次の取り直しでまとめる・取り直したら coord の再点検と fed の固定値の書き換えが要るので fed に知らせる): S1b の警告に「塗装は NS 形のとき」を添える / overlay の E2 の stop_reason に STOP_SOURCE_INACTIVE を入れる(fed は ROW_EVALUATOR_STOPS から ROW_STOP_REASONS へ移す)。fed の固定契約 ft0_contract.py と抽出器の一致は `test_t0_contract_crosscheck.py`(T0_CONTRACT_PATH で場所を指定可)。overlay の形の独自決定 7 点(stop_reason をリスト・leads_to・alt_conditions・UNCONFIRMED・condition_note の言い換え・safety の stop_reason/authority_note・binding の from の形)は fed が全件採用。段階 2(対外)は別承認。

関連: [[fault-tree-stage3-fed]]・[[hash-match-not-completeness]]
