---
name: neo4j-dumps-delivery-20261022
description: 顧客(JR東海)提出用の 4 グラフ(B/C/D/語彙)の Neo4j dump 一式と構築・load 手順書を 2026-10-22 に作成・納品フォルダは git 外
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-10-22T01:40:31.891Z
---

2026-10-22(利用者承認)に、顧客提出用の dump 一式を作った。

- 置き場(git 外・本体ツリー): `knowledge_kb_v8/data/delivery/neo4j_dumps_20261022/`(dump 4 本・MANIFEST.md・SHA256SUMS・counts_expected.csv・verify_counts.cypher・docker-compose.example.yml・README.md・zip)。ARTIFACTS.md に登録済み。
- 手順書(git): `docs/顧客提出_Neo4jダンプ_構築とload手順_20261022.md`(README.md と同文・Community 4 インスタンス / Enterprise 1 インスタンス 4 DB・Docker/非 Docker/Windows・照合・よくある失敗)。
- 取り方: 手元 4 コンテナ(9890/9990/10090/10190)を 1 本ずつ停止(5〜7 秒)→ `neo4j-admin database dump neo4j --to-stdout` → 起動(kb_backup.sh と同じ方式)。LOCKS 予約と info 予告/終了あり。
- 検証: ラベル別・関係型別・総数 263 行が手元と EC2 で全一致。復元試験 = 空ボリュームへ load → 起動 → 総数と制約数が一致(4/4・16〜18 秒/本)。
- 版の事実: 4 本とも Neo4j 2026.03.1 Community。APOC は B と C のコンテナだけ(手元も EC2 も D・語彙には無い)。語彙 DB は 11,396 ノード(10/17 返答案の 7,840 は古い)。
- dump に含まれないもの: system DB(ユーザー・パスワード)・原本・マスク前の文・マスク辞書。B はマスク済み本文を含む(届け先はデータ所有者)。

**Why:** 顧客の「CSV 取込みで構築できるか」への返答(10/17 返答案 案 1 = dump/load 推奨)の実物を用意するため。
**How to apply:** 次に dump を作り直すときは同じフォルダ名の日付違いで作り、counts_expected と復元試験を必ず回す。顧客の Neo4j の版が 2026.03 未満なら load 不可なので、版を先に聞く。関連 [[kb-excel-export]]・[[local-backup-setup]]・[[dump-load-preserves-storage-order]]。

**2026-11-19(決351 追補 111)**: 渡した書き出しは、伏せ直しの前の版(伏せ漏れ 92 か所を含む)。利用者の決め = 今は顧客に連絡せず、次に書き出しを提出する時に、伏せ直した版で置き換え、前の版の廃棄のお願いを添える。
