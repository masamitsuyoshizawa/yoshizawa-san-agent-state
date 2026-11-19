---
name: dify-button-acceptance-r290-bkb
description: 決290(Dify 探索木のボタン回答・human-input)の bkb の受入 — 限定付き合格(rep 0101)→ 追補 7 差分 合格(rep 0156)→ 決293 反映済み → 決295 別の質問にする 差分 合格(rep 1439・a13eeba4)
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-10-22T12:57:14.725Z
---

2026-10-22 coord req 2145(決289/290・利用者承認)。受入設計 `docs/設計_受入_探索木_ボタン回答_bkb_20261022.md` 改訂 0(723a64e859944f18・コミット 41cc4e92)。

- 単位 = 段(tool の 1 回)。期待の正本 = 状態整理の規則 1〜7(EC2 系統の D の DSL ec2_diagnose_fixed.yml 3e553d7ead47a030 の状態整理の節点 3002 の system の文)・fed 設計 改訂 3 d5ac22c4a9bf289d・プラグイン dkg_diagnose.py f66f278a040d2059(with_tree の道 = POST /v1/dkg/tree 1 回)。G は ec2_guided_fixed.yml 55c8dd6466c340b6。
- (a) bkb_apply_button を規則から独立に書く(測定の normal/abnormal は asked を変えない・4a は置き換えて asked は重ねない)。カードの質問 = 直前の tool の json の tree.children の先頭(図の並び)。(b) fed の simulate の tool を本物のプラグインの _invoke で回し、口の直呼びと除外 6 欄の後に byte 同一。(c)〜(g) と場面 12 は設計書。
- 分担: EC2 の本物のボタン・止まる/再開・古いカード・画面は coord。bkb は coord の記録に同じ照合を当てる。
- 改訂 1(2026-10-22 22:14・rep 2214・e6de5bf21e89cea8): rev R290-2 = (a2) prev は段 k のボタン前の観測・received(appended/no_change/訂正・prev 誤りの負例 3 は (a2) でだけ落ちる)。R290-1 = (h) A の段の節点の境目すべてに B の保存を差し込み、守れない境目が「照合直後・保存直前」の 2 か所だけかを数える。G1 正例を厳密に + 負例 11。(i) カードにならない負例。測定 unknown は暫定で free_text 扱い → 結果は「限定付きの合格」。
- fed の実装 = fed-btn c26893b2(生成 dify_button_layer.py・EC2 用 DSL ec2_diagnose_button.yml 376858f03ded8229・fed 設計 改訂 4 249627ece205bf54)。ただし R290 の是正(coord req 2211)はこれから → 是正の版で受入を回す。
- 改訂 2(2026-10-22 23:24・rep 2324・c536de00c23d583d): fed 改訂 5(65c0d424・fed-btn 16f90358)= 整形 → tool → 番号表 → 照合 1 点 → 1 つの assigner で 4 変数。古いカードでも tool は呼ばれる → 判定は会話変数が不変か。割り込みは差し込み方 3 つ(一式・札だけ・3007 だけ)× 境目すべてで「守れた/守れない」を数える(rev の GET 後保留は守れない側・守れるとは書かない・assigner ≠ DB 原子性)。R290-3 = available 偽 / meta.available 偽の合成でカードにしない。古いカードは段 2・3 も(S11b)。probe は log.json の中身で判定。
- 落とし穴: 改訂の記録に前の版のコミットを書くときは git log から取る(改訂 2 で一度でたらめな値を書いて直した)。
- **手元の受入 限定付き合格**(2026-10-23 01:01・rep 0101): 対象 fed-btn c7689332(生成 9ee2b95e・模擬 c1639515・設計 改訂 6 498f02db)+ main b0029b02 の kg_api + プラグイン 0.3.18 0f0811b2。道具 button_r290_acceptance.py(prep/run/mut・コミット 373226ea・本物のプラグインを TestClient の口へ・fed の Run を台に・期待は独立)。照合 44 項目合格・変異 13/13・割り込み 30 境目 = 守れた 19/守れない 9/保存の後 2(限定どおり)。
- 要判断: 答えた測定が図の先頭に残り同じカードが続く(例題 1 ms:0498 基準外 ×3・公開 1 問 ms:0559 正常)→ 案 (i) 取り出しで飛ばす (ii) dkb で外す (iii) 注記。
- 落とし穴: 写しの kg_api の隣に profiles 等のリンクが要る(無いと D-KG の準備が全部失敗)・fed の code_of は関数を返す・同じ答えの置き換えは received no_change・不出の照合はカードで足した部分に当てる(本文の出典の事故 ID と原本 PDF 名は今の作り)。
- 要判断は案 (i) に決まった(決290 追補 8)。**追補 7 差分 合格**(2026-10-23 01:56・rep 0156・fed-btn 66aa8e5c・生成 a6fe7297・模擬 a8827b58・設計 改訂 7 c0733954・BTN_TARGET=66aa8e5c): 繰り返し 0・カードの質問 55/55・取り出しの単体 5/5・44 項目そのまま・DSL の差は取り出しの 3 節点だけ・変異 14/14。決293(本番の D へ反映)の条件 = この合格。
- 決293 反映済み・決295 の受入設計 改訂 3(2026-10-23・本番 D = ボタン版 38d8fe2a・プラグイン 0.3.18)。EC2 の記録(probe の log.json 4 本)を bkb が読んだ(info 0205): 本番の既定 3 段・失敗 0・基準外の後は別の質問へ進む・古いカードは画面に出ない(notes の行は成功の記録)。
- 決294/295(2026-10-23): 「わからない」→「別の質問にする」(id other・tool を呼ばず同じ図の次の未回答・答え済みと skipped を飛ばす・段をまたいで持ち越し・次の実行に持ち越さない・差し替えカードに図なし・上限 2・asked に足さない)。受入設計 改訂 3 §3 (j)(d4f00ae4 系・rep 1040)。測定 unknown の暫定と dkb req 1938 はボタンの道では不要に。
- **決295 差分 合格**(2026-10-23 14:39・rep 1439・fed-btn a13eeba4・生成 f27b9cce・設計 改訂 9 45abb477・BTN_TARGET=a13eeba4・道具 28b49c51): 全項目 56・変異 19/19・DSL 08aeae38 = 作り直しと byte 同一。カードの順の照合(check_flow)で、差し替えは tool を呼ばない。落とし穴: 集約を全部古い順にすると答えが付くのでなく other が整形へ届いて止まる → 変異を m15a(中身だけ)/m15b に分けた・持ち越しの照合は「差し替え済みを数えなければまだ出る」で中身の無い合格を防ぐ・k_no_repeated_card は今の流れでは中身の無い合格。req 1044/1038 閉じ。決296 で本番 D = 08aeae38(2026-10-23 15:20・coord)。EC2 の記録を読んだ(info 1521): log.json 5 本 failed 0・**probe の has_figure は図のあるカードでも全部 False**(中身の無い値)→ 図の有無は画面の png で見る。bkb の手番なし。

**Why:** 決270 以来と同じく、期待は上流(規則の文・tool の json)から作り、実装の関数は使わない。
**How to apply:** fed の実装の知らせが来たら、設計 §2 の場面を固定の写しで回す。関連 [[tree-v3-acceptance-r275-bkb]] [[dify-human-input-facts]]
