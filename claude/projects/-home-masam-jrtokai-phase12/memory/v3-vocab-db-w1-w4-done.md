---
name: v3-vocab-db-w1-w4-done
description: V3 第 2 段の語彙 DB(dkb)— S7 完了(EC2 複製・vocab_ec2)・W1〜W4 完了・正本に v3.0 投入済み(束の digest 2e763fba・2026-09-24)・残りは R2 以降の経路と否定例・道具の置き場と使い方
metadata:
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-09-24T04:04:00.202Z
---

2026-09-24、決46(利用者承認)で S3 実装に着手し、同日に W1〜W4 を終えた。

- **正本の語彙 DB**: コンテナ `neo4j-vocab-v3`・bolt 10190・http 10177・compose `deploy/docker-compose.vocab.yml`(project 名 `jrtokai-vocab-v3`)。**v3.0(release_ord 1)を投入・公開・登録済み**。束の digest(`vocab-bundle-digest-v1`)= **`2e763fbae25c6a3d`**(ノード 7,840・関係 9,549・要素 17,389)= 演習 = 独立計算 = bkb の独立投影。台帳 `graph_registry.json` の `vocab` 鍵(vocab_change_count 0・max_release_ord 1)。
- **設計書は追補版 10 `97baf15301bfa242`**(承認版 追補版 5 から 8 行の書き換え・行は足さない運用で F/B の行番号を守る)。決46 追補(空の null_fields 拒否)・決49(正準名は NFKC・444/67)・決50(R1 は VocabChange を作らない)・決51(Equipment に source/status/basis)。
- **道具**(`knowledge_kb_v8/scripts/vocab/`): `vocab_model.py`(入力 4 本 → 論理モデル)・`vocab_db.py`(表し方・復元の札・2 相の書込み・`read_bundle`)・`vocab_bundle_digest.py`(`--export` / `--digest`)・`vocab_gate.py`(G0 の検査 1〜8・G14・G15)・`vocab_schema.py`・試験 `test_vocab_codec.py`(G0 の門に載せる・件数は固定しない)/ `test_vocab_model_counts.py`(辞書の版に固定・門に載せない)/ `test_vocab_w1.py`。
- **共有スクリプトの積荷 `vocab`**: `ingest_pipeline.py --payload vocab --vocab-release vX.Y [--init-vocab]`・`--register-digest --targets vocab`・`rehearse_env.py --payload vocab [--vocab-fault phase2|registry]`(一時 DB 18690)。台帳は `vocab` の中にだけ書き、最上位の ts/stem/git_head は触らない。
- **接続**: パスワードは `docker inspect <container>` の NEO4J_AUTH から値を出さずに渡す。投入は本体ツリーから(worktree の `knowledge_kb_v8/data` は本体へのリンク)。
- **R2 以降の経路も済(コミット aea5d0c5・2026-09-24 13:21)**: `vocab_diff.py`(差分の計画)・決53(失敗版のノードは第 1 相で since を書き換えて再利用・閉じたノードは第 2 相で until を外して復活)・決54(読み手の札の閉じた一覧 16 = `READER_TAGS`・第 2 相で辺の端点が束に在ることを確かめる)。筋書き `rehearse_vocab_scenarios.py`(一時 DB 18691)44 件 NG 0。設計書は追補版 11 `638f122e3635f4ed`。
- **実測: Neo4j 2026.03.1 は read committed** — 1 つの read transaction でも途中の公開の commit が見える。**束の読込みは読み終わりに P を読み直してやり直す**(`BUNDLE_RACE`)。
- **S4 は決57 で終了(2026-09-24 13:31)**。設計書は追補版 12 `47b3d25645d5ab55`(決56: 読み直しの上限 3・札 17)。S4 の記録は info 20260924-1332。
- **S6(bkb)の備品**: `rehearse_vocab_scenarios.py` の `--prepare-r1` → `--inject-case SPEC CASE DIR`(1 例を生のプロパティで書く)・汎用の `--inject SPEC DIR`・交錯試験用の `--prepare-6f` / `--publish-6f`・片付け `--drop-6f`。一時 DB は 127.0.0.1:18691。拒否は `vocab_db.VocabError` の `.tag`。bkb の往12 で参照実装の誤り(Predicate の条件付きの欄の欠け → 正しくは `origin_field_mismatch`)が見つかり是正(`adb6de72`)。
- **共通表 第 3 版 改訂 11 確定(決66・2026-09-24 16:20)**: `docs/設計_V3_共通の定義と承認事項_20260921.md` sha16 30ba93598a6da687(842 行)。以後は「§n + 改訂 11 30ba9359」で引く。**語彙 DB の設計書は共通表を 107 行で引いている → 次に設計書を改訂するときに引く版を改訂 11 へ更新**。第 2 段で dkb の手番は終わり。
- **S7 完了(決63・2026-09-24 15:24)**: EC2 の V3 API は neo4j-vocab(v3.0)を読む。EC2 の新設資源(neo4j-vocab・vocab_data・port 10190)は残置。C:3/C:10 は 12b57a5d で是正(vocab_copy_check 49c26406)。次の宿題 = 共通表の次の改訂の項目 54〜69(coord が段取り)。
- **S7 は S7b まで合格(2026-09-24 15:20)**。**W7 minus-d の複製 5 本を作った(15:24・コミット 9c129aed・道具 `knowledge_kb_v8/scripts/dict/make_minus_d.py`)**: 置き場 `knowledge_kb_v8/data/dict/minus_d/`(git 外)・`dict_equipment__minus_<d>.json` と `expect__minus_<d>.json`(bkb の run_v3_dict_pair --expect の形)・214/429/366/451/439・bkb の道具で 5 本とも不合格 0。凍V-c の測定は bkb。
- **S7-A の dkb 分 完了(2026-09-24 15:05・rep 1505)**: EC2 `neo4j-vocab`(127.0.0.1:10190・トンネル `ssh -N -L 19892:127.0.0.1:10190 kb-demo-ec2`・資格情報はローカル B `neo4j-accident-v7` の NEO4J_AUTH と同じ値)を 24 項目一致で確認・台帳に `vocab_ec2`(uri_host localhost:19892・他の鍵不変)・E4 RELEASE_EXISTS・E5 (a)(b)(c) 合格。残りは coord/fed の S7a/S7b。
- **S7 承認(決62・2026-09-24 14:51)後の dkb 分 (1)(2) 済み**: 正本の dump `knowledge_kb_v8/data/vocab/s7_dump_v3.0/neo4j.dump`(sha256 3cf69f1b87b21375…・2,152,848 B・止め 13.4 秒・前後一致)・元の値 before_v3.0_E1E3.json(db9b64c1…)・kb_conn 確定版 94f359da216d1e8a…(a7c7c872・CANONICAL[vocab] に neo4j-vocab:7687 / localhost:19892 / 127.0.0.1:19892)。残り: coord の EC2 複製の合図 → トンネル越しに E1〜E3・E5 → 台帳 vocab_ec2(LOCKS・0-2 で不在確認)→ E4。C:3/C:10 は次の改訂。
- **S7 計画 第 3 版 `4db9e33769419e6d`(コミット ba68b1b1・決61 反映)**: 搬送=sha256 全値・稼働 DB=論理データ+スキーマ定義(バイト単位の同一は撤回)・E3 は定義一覧の一致と各側の単独要求(同じ欠け同士を通さない)・§2-0 事前照合・新設資源だけ削除・E4 は g0_vocab(v3.0・入力なし)が RELEASE_EXISTS で止まれば合格・E5 はトンネル先を設定値(EC2 512MiB/256MiB vs 正本 1GiB/512MiB)で区別。道具 vocab_copy_check d54cee94。
- **S7(EC2)の計画 第 2 版** `docs/計画_V3_第2段_S7_EC2語彙DB_dkb_20260924.md` `ef7b40355bcb1630`(初版 57186286・dump → load で複製・vocab_ec2)。**EC2 の neo4j-vocab の認証 = kg_api の KG_NEO4J_PW と同じ値**(fed 案 B の P2・値は書かない)。kb_conn の CANONICAL 加法は S7b の前に EC2 へ配る。**承認まで EC2 に触らない**(coord が S6 合格・fed の切替計画 改訂 1 fc95c947 と 1 便で諮る)。
- **S7 の備品(コミット 7aafe0b0)**: `vocab_copy_check.py`(E1〜E3 を `--snap --uri` で読み取り・`--compare` 15 項目)と `vocab_dump.py`(`dump` は `--execute` 必須・ポートと容器の一致を確かめてから止める / `load-rehearse` は 18692 の使い捨て)。一時 DB で通し試行合格。**`--user` で dump すると database_lock に書けず失敗**。正本の全件 hash = 6de974dc7fa15b8a(v3.0・14:06)。同じ入力で別に投入した DB は E1 一致・E2 不一致。
- **S6 の往 23 例は全件 PASS**(bkb rep 20260924-1357)。備品は起動に失敗したときに状態とログを残す(d5ea0f26)。

関連: [[v3-stage1-design-approved]]・[[negative-test-check-reason]]・[[empty-set-is-subset-of-anything]]
