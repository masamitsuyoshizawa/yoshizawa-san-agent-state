---
name: dkb-coverage-singularity-bkb
description: 決80(D-KB 検索カバレッジ拡大)の bkb 分の実測結果と、場所・線区のマスク前の値の要判断(2026-09-25)
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-09-25T07:49:53.462Z
---

決80(2026-09-25・利用者指示)の bkb 分 = 草案 1 §5 の 4。rep 20260925-1649(needs_user_approval: yes)。台 run_dkb_coverage_singularity.py(ea4ffbe4・コミット 5a579dcc)。

- 母数 dkg_loo700_cases.jsonl 647 問(基準値の母数 566 の定義は特定できず)。自分を除いて > 0: 設備 560・場所 89(双子も除くと 79)・同じ設備 0(番号が在る問は 5 だけ → LOO では測れない)。
- HTTP なら 49 問が上限 90 で 422。場所の NFKC の限定は 159 問で新規ヒット 0。
- 原因コードは B に無い。近いのは CauseConcept(475/700 事故)。Cause.text は原文相当。
- **要判断**: Accident に place_text_masked 22・line_text_masked 30(計 47 事故・マスク側にだけ「[氏名]」)。facets@v4(EC2 含む)は生の値を返す。消された部分 4 種・2 文字・すべて B の地名に含まれる(地名への誤マスクの推定)。
- マスクの経緯を調べる操作は自動判定(PII)で止められた。**同じ調べを別の手段で続けない**。判断は利用者へ。

**Why:** 個人情報は最優先。値を会話・連絡文に出さず、件数と欄名だけで報告した。
**How to apply:** 場所・線区を出す設計(D-KB の注意喚起など)の前に、この要判断の結果を確かめる。関連 [[v3-stage2-s3-plan-state]]。

**2026-09-25 17:00 決81 諮5 = 判定までマスク側を返す(安全側)**。bkb の rep 1700: 対象 47 事故の id と欄・規則案(_masked 非 null ならそれ・空でも生へ戻さない・照合は変えない)・生を返す口 = facets@v4 / accident/search / accident/doc(全文も) / federate B 枠(線区と駅・踏切・区間の name) / {graph}/ask。返さない = clues / diagnose / related_cases_B。Section.name_masked 12 も在る。(3) 欄の意味と経緯は調べない(自動判定で止められた)— 利用者が手元で見る手順案を出した。

**2026-09-25 17:07 追補 1・3 の後**: search・doc の実装計画 改訂 1(f530d79e・coalesce で 4 箇所・承認待ち・dkb へ共有ファイル合意 req 1704)。47 事故の全文側: 自分の消された部分は 7 欄とも 0(Document.text・extraction_json・notes・summary・Cause.text・FailureMode.text・source_file)。他の事故の 4 種は Document.text と extraction_json に各 2(地名の可能性・利用者が見る対象に加える推し)。difflib の差分は 1 文字の断片(延べ 21)が混じるので、「4 種」は 2 文字以上で数える。dkb req 1703(CauseConcept の経路)は rep 1706 で回答・閉じた(Node ラベル無しの概念 12 に注意)。

**2026-09-25 17:14 決82 の bkb 分を実装(e70f35a6)**: accident_backend.py 7c104c43(coalesce 4 箇所・dkb 合意 rep 1709)・試験 test_accident_mask_fields.py 7/7(否定 = 規則を外した変異で RAW 52)・共通の検査 mask_field_check.py e30c20f9(自己試験 12/12・--facets/--search/--doc/--federate-file・RAW 1 件で rc 1・0 件は未検査 rc 2)。変更前の EC2 facets(S7b)で RAW 72 を検出。次 = coord の EC2 反映後に search・doc の応答へ同じ検査。

**2026-09-25 18:03 検 C5(dkb req 1752・rep 1803)**: location_class は strict だけ 73 / strict + grouped 79(差の 6 問は grouped の組だけ)。双子の範囲(直接の相手 / 同じ正本の群)は数を変えない。acc:0556 はどの集合にも入らない。台 run_dkb_c5_singularity_sets.py cfc32549。教訓: re: を推測の便名で書いた(ls で実物を見る前に書いた)→ その場で直した。

**2026-09-25 18:12 決85(info 1812)**: 89(bkb・strict+grouped・自分だけ)と 85(fed・strict・自分だけ)の差 4 は grouped だけで当たる問。版 dfe4ce39 と e86b399e で 89 は要素まで同じ。双子も除くと 73 / 79(2 問が grouped だけへ移り差 6)。fed と数が一致。どちらも facets を通すので facets 自体の独立照合ではない。

**2026-09-25 18:43 決86 = 利用者判定: 47 事故 52 欄・名前 21・8 種はすべて地名(人名 0)→ マスク側を返す規則を撤回**。bkb: accident_backend.py を 39c75c96 に戻した(6ec42a4c)・試験 6/6・mask_field_check は --expect raw が既定(masked で旧向き)。search の text は要約を返し、要約はマスク済み(36 行)= 正本の是正 (2) の対象。(2) 正本の是正: dkb が棚卸し・除外語・ingest_pipeline 投入・*_masked の扱い、bkb は受入検査 受M1〜受M6(受M2 = 欄ごとの [氏名] が棚卸しの数だけ減りそれ以上減らない)。対象の一覧は coord の判定材料をそのまま(bkb は作り直さない・差分では 4 種/8 種で一致が不明)。**bkb はマスク処理の中身を読まない**(自動判定の件)。B の [氏名] は Document 530/700 など広く、多くは本物の氏名と推定(dkb)。47 件の外に place_text の [氏名] 63 件(判定の範囲外)。

**2026-09-25 19:15 決88 の受入の準備**: 台 canonical_fix_acceptance.py(0a49957d・自己試験 9/9・値は持たない)・是正の前の基準値 before.json eedb1779(B 89fea892)。**rep 1910 §1 を訂正(rep 1915)**: 「一覧に入らない 4 文字」は印 [氏名] そのもの(生の値にも [氏名] が残る 4 か所 = acc:0609・acc:0456 の place_text・区間名 2)→ 一覧 v2 から外すのを推した。真の元の文字列は 8 種・延べ 75(coord の 77 と要照合)。id は据え置き(決88 追補 1)。place_text の [氏名] 63 = 判定済みの外 58 + 場所に別欄 2 + 線区だけ別欄 3。

**2026-09-25 19:24 決89**: 一覧 v3 = 71349dd0(8 種・印を外した)を確認。基準値は before_v3.json bf7a5a19 を使う(旧 eedb1779 は 9 種で使わない)。受入の台 b60d9b4d: 受M1 = 69 欄(73 − 生にも印が残る 4 欄)・受M1b = 4 欄が不変。次 = dkb の見本(判1・種ごとに利用者が決める)と棚卸しの数 → 受M2 の --inventory → 演習の後と投入の後に回す。

**2026-09-25 21:09 決95 S4 の受入(写し 18687)= rep 2109**: 受M1 69・受M1b 4・受M2 20 欄(合計 2,379)・受M4・*_masked 撤去 73 → 0 が PASS。受M6 は決94 で範囲が広がり置き換わった(8 = place_text の減り)→ 台の定義を「範囲外の変化 = 位置の表の place_text の数」に直す宿題。dkb の「前」= 当方の基準値(20 欄)。graph_digest は実装が 2 つ: bkb-gd-5 で正本 89fea892・写し 20aaa1e4 / export_graph_apoc で正本 f572fce7・写し 360406c6。source_file の [氏名] 128 は対象外で不変。次 = S5(本番)の後に受M1〜M6 と受M5(API)。

**2026-09-25 21:33 決96 S5 の後の正本の受入 = rep 2133**: 受M1〜受M6(受M6 は決96 の 3 条件)・受M5(accident_backend の doc・search を 9890 に向け sha16 で照合・facets/federate は未)すべて PASS。正本 bkb-gd-5 = 20aaa1e4(写しと同じ・apoc では 360406c6)。audit_dkb --all --loo700(本体ツリー)= PASS 10 / WARN 4 / FAIL 3(X1 = dkg_backend.py の中に gold の窓・X3r = 静的 (b) 3 か所・X17 = 既存の投入物)で、どれも S5 と別。S5 前の監査記録が無く、いつからかは未確認。次 = S6(EC2)は別承認・その後に EC2 で受M5。

**2026-09-25 22:02 決97 S6 の後の EC2 の受入 = rep 2202**: EC2 の B(トンネル 19890 → 172.20.0.2:7687・~/.kb_ec2.env の KB_ACCIDENT_NEO4J_PASSWORD)で受M1〜受M6 PASS・bkb-gd-5 20aaa1e4 = ローカル S5 後。受M5 はコンテナ内の run_ec2_unmask_m5.py(sha16 と件数だけ)で doc 50・全文 47・search 20・facets 21 が生の値と同じ(facets 24 事故は 422/未当たりで未検査)。台の正本判定を部分文字列からポートの完全一致に直した(19890 を誤って止めた)。基準値は ローカルの投入前(EC2 の投入前は直接は測っていない)。

**2026-09-25 22:28 決98 の bkb 分 = rep 2228**: facets の照会 4 種は ORDER BY … a.id DESC LIMIT で保存の順に依らない(facet_backend は開かず受け皿で照会の文を集めた)。accident_backend.search は ORDER BY score DESC LIMIT $k・同点の第 2 キー無し・丸めた得点で並べる → 潜在の依存(案: a.id を第 2 キーに・生の得点で並べる・共有ファイルなので dkb と合意・別承認)。実測(ローカル正本 ↔ EC2 B・同じ中身 20aaa1e4・666 問)で facets・search とも差 0(この一対で保存の順が違うかは未確認)。台 run_order_dependence_facets.py 0a628736。
