---
name: intake-demo-r350-bkb
description: 決350 事故摘録の追加のデモ(bkb 分)= 見本と処理役の設計 改訂 0・G8 の故障様式の照らしの欠陥・演習の木の G8 追加が実装ツリーへ書く道(2026-11-05)
metadata:
  node_type: memory
  type: project
  originSessionId: 3ec39d96-ff9c-471f-807d-b015427a444c
  modified: 2026-11-05T02:56:22.184Z
---

2026-11-05、bkb は決350 段 1 の設計 `docs/設計_事故摘録の追加_デモ_bkb_20261105.md` 改訂 0(sha16 0e2ffe243a74d694・main e5ef9d9e4)を coord・dkb・fed へ rep 20261105-1153 で返した。台は演習のモードが既定(決350 追補 1)。

- 見本 = 架空 2 件(甲・乙)。区分 軌道回路、人名 見本太郎・架空花子、駅 模擬駅。置き場 = git 管理の `knowledge_kb_v8/scripts/bkb/intake_demo/samples/`(manifest.json に sha256)。まだ作っていない(段 2)。
- 処理役 = rehearse・cap 1.0。Stage1 の maxTokens 32000 で、予約の上界は 0.842 USD。
- summary.json は done の前に書かれる。写しの B に事故が入るのは G3 の適用。
- **G8 の欠陥(確定)**: `known_fm` が surface_map(dict の一覧)を dict の前提で読み、語を 1 つも登録済みと認めない。既存 702 件のうち 648 件が G8 で止まる側になる(surface を読めば 170、confirmed だけなら 502)。是正を段 2 に入れるかは coord 経由で諮り中。直すと、dkb の受入の偽の表で G8 の待ちを起こす形の差し替えが要る。
- 演習の木で G8 に追加つきの承認をすると、`apply_dict_additions.py` が abspath から書き先を組むため、記号リンクを通って実装ツリーの git 管理の辞書へ書く。`--commit` の G10 の `backup_llm_artifacts.py` は、本体ツリー(決め打ち)を読み /mnt/d へ書く。

- 2026-11-05 12:00 dkb は rep 1153 を全部採った(設計 改訂 3 49dc69357198e82a)。G8 の書き込みは、台の元を専用の作業ツリー(detach)にして避ける。決343 段 2(10/31)で G10 の退避が実際に D: に世代を作っていた(dkb info 1158・利用者の判断待ち)。G10 の手当ての持ち主は、bkb を主にすると答えた(rep 1202)。最初の案(commit かつ kb_conn.is_canonical)は dkb の指摘で取り下げた。EC2 のトンネル 19890・19891 は台帳(b_ec2・dkg_ec2)には在るが、kb_conn の CANONICAL には無いため、EC2 の commit で退避が黙って模擬になる。今は dkb の案 2(ROOT が本体ツリーのときだけ退避・git common dir の親で判じる)に賛成している(rep 1207)。案 3 の Fail は G0 の台帳の照らしと重なるので推さない。道具の側の git の照らしは、記号リンクの先が答えて効かない。解かない道の根で問う。KB_CONN_MODE と DKB_MAIN_TREE はどちらも目印にならない。coord への req 1205 で利用者の判断待ち。

- G10 の手当てを実装するときのメモ(dkb info 1210 と bkb の返事・11-05 12:1x): 工程の ROOT は Path(__file__).resolve() から作る。台の木に留まるのは、make_tree が accident_kb_v7/scripts を写す(リンクにしない)からである。worker は工程を解いた道で起動する(runner.py :95 resolve)ので、abspath で作っても守りにならない。試験は「scripts がリンクのときも退避しない」を worker と同じ起動の形で書き、make_tree が scripts を写すことの照らしと対にする。道具の側の (C) は REPO を abspath か parents[3] で作り、resolve と realpath は使わない。

- 決350 追補 2(11-05 12:12・利用者): G8 は段 2 で直す(提案も確定も登録済み)。設計 改訂 1 c8c90fbaddbe2dcb の §6-1-1〜3(試験 K1〜K8・dkb の base を先に替え、g8pass は是正と同じ便)。G10 の案 2 は実装済み: main ac196c4db・工程 10da6f9271b06a40・test_g10_backup_gate 10/10・show-toplevel が REPO そのもの・GIT_ を落とす(rep 1223・rev の差分確認待ち)。§6-2 は W2 の前の項目。D: の世代 20261031-1322 は利用者の判断で消した。残りは、記録の再生をデモに使うか。rev の点検の組は改訂 0 で固定。

- 決350 追補 4(11-05 13:15・rev の要是正): 設計 改訂 2 4d34ba98177ba733(rep 1325・対応表は §10)。R350-1 = worker の precheck で manifest を照らす。鍵は sample_manifest と sample_manifest_sha256、札は INPUT_NOT_IN_MANIFEST。古い worker は知らない鍵を無視するので、心拍の input_guard で台に見せる。R350-2 = max_jobs と札 JOB_LIMIT_REACHED。数えるのは pick の出来事と resume_launch の和(state を先に書くため)。R350-3 = fake_llm_sha16 の値で判じる。manifest は bkb の intake-demo-samples/1 が正で、鍵は 4 つ(schema・sha256・category・file)。**件数の訂正**: G8 で止まる事故は、今 690・故障様式だけ直す 670・別名だけ直す 669・両方直す 446(648 → 170 は故障様式の側だけの数だった)。dkb の base の「絶縁材」はどの直し方でも未知。g8pass の設備は正準の語だけ。設備の別名の是正の採否は coord が諮る。

- 決350 追補 5(11-05 13:30・利用者): 設備の別名も段 2 で、故障様式と同じ便で直す(dict なら surface・形の照らし・K10・K11)。設計 改訂 3 は 00b60bc5429a1aa6(rep 1332)。札 2 つは §2-10 の SHARED の行に足す(10 → 12)。W1 設計 改訂 25 として、段 2 の fed の契約と同じ便で入れる(rep 1330)。rev の次の確認は bkb 改訂 3・fed 改訂 5・dkb 改訂 4 の組。

- 段 2 の実装(11-05 21:15・rep 2115・main 2cfd78a4f / b9709db86)。G8 の是正は c6169b46f(工程 cf82c9d6・test_g8_dict_known 16/16・K8 = 446)。見本と偽の表は intake_demo/(manifest f0d1069c・verify_spans で hard 0)。worker は sample_guard.py(precheck で INPUT_NOT_IN_MANIFEST・start_job で JOB_LIMIT_REACHED・input_guard は鍵があるときだけ・M6 29/29・変異 82/82)。W1 の設計は改訂 25(SHARED 12)、デモの設計は改訂 4(1d9957ee・§11 に読み替え)。台の上の通し T-b〜T-g は、dkb の台に bkb の照らし check_demo_rig_bkb.py(読むだけ・-B)を当てて 6/6(11-05 21:40・rep 2140)。bkb の段 2 は閉じた。残りは、rev の実装の確認と段 3(本物の LLM 1 件)。

- 決350 追補 10(11-06 02:02・rev の実装の確認で要是正 R350I-3・rep 0202・main 91e46605b): 保持の片付けが request.json を消すと、max_jobs が過去の起動を忘れていた。直し = 起動の前に jobs_dir/.worker.started.jsonl へ追記し、台帳と置き場の和で数える(request.json を条件にしない・読めなければ止める・今のジョブは最後に除く)。M6 は 37/37(S10 = 本物の retention.prune の後の否定例)。intake_mutants.py は判定の前に落ちた物を捕まえたに数えず、--json-out で変異ごとの区分を残す。86/86 を判定で捕まえた(記録は git 外 knowledge_kb_v8/data/eval/bkb/intake_mutants_20261106/)。設計は改訂 5(9125b97c)。G8 は合格。

- 段 3(11-06・dkb rep 1457): 本物の LLM の甲は G8 の needs_dec で止まった。故障様式の「軌道回路が落下」「絶縁不良」が辞書の surface に無いため。費用は 0.05309 USD。決350 追補 14(利用者): デモは 2 つ並べて見せる(本物の記録の再生 = G8 で止まる / 模擬の応答 = 結果の欄まで)。案 2(見本の言い回しを替える・bkb info 1459 の「軌道短絡が発生」「レール絶縁不良」)は今はしない。材料は残す。台が動いている間は、SendMessage だけで受領し、コミットと本体への早送りは台が止まった後にする(coord が続けるよう言った)。

**Why:** 段 2 の実装の承認と rev の点検がこの版を基準にする。G8 の欠陥は W2(正本の登録)にも効く。
**How to apply:** 段 2 に入る前に coord の判断(G8 の是正の採否・記録の再生の採否)を dec で確かめる。工程を変えるときは LOCKS を取る。関連: [[next-phase-challenges-ab]]・[[intake-accident-sat-registration-item]]・[[claim-wider-than-implementation]]


(索引から移した説明・2026-11-07)
- [決350 デモの見本と処理役(bkb)](intake-demo-r350-bkb.md) — 段 2 は済み・rev の R350I-3(片付けで max_jobs が忘れる)を直した(main 91e46605b・起動の台帳・変異 86/86 を判定で・設計 改訂 5)・次は rev の再確認と段 3・G8 で止まる事故は 690/670/669/446(648→170 は故障様式だけの数で訂正)・G10 案 2 は実装済み ac196c4db
