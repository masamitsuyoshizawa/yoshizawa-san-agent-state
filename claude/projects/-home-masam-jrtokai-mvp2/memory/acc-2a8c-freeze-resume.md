---
name: acc-2a8c-freeze-resume
description: 図面 3 枚の期待は凍結済み(b08e は 2026-11-04 01:44・info 0145・74a40af)・残りは自己点検の直しを諮る・f12fc の B4(照合〔C 層〕の計画と旧方式の基準線は決48 0-8 で済み・[[acc-yardstick-phase0]])
metadata:
  node_type: memory
  type: project
  originSessionId: b54d92f6-694f-48da-9c1b-bc1712afcb52
  modified: 2026-10-31T15:55:30.730Z
---

2a8c の期待は、2026-11-01 00:53 に凍結した。
- expect_v1.json の sha256 は 055809d67b484f920b6d7ccc03f8b4fd14fe17d8dab8b96737b7661e2a8e298b。
- info は `20261101-0054_acc_to_coord_dgr_info_2a8c-expect-frozen.md`(コミット fce6874)。宛先は coord と dgr。
- 利用者の判断(decisions.json)は、不確か 1 節・決まり 3 節。B6 は aid3 でやり直し、利用者が「自信がある」と答えた。

**残り**
1. 済み(2026-11-01): 利用者が「下書きを消す」を押し、acc が 8731 のサーバーを止めた。entries は 18 行のまま(sha16 46c6a2b868f2c71b = 凍結の出所と同じ)。
2. 済み(2026-11-01): 決33(不確かの節はどの図でも合否に入れず報告に数を併記・数え方の決まりは一般の規則で設計に)。設計 第 9 版・検討記録 §26・記入票の説明(d013602・699fcb2)。照合(C 層)の計画に決33 §2-1 の 2 と 4 を入れる。
4. 済み(2026-11-01 14:5x): A 層の道具を probe_v2 にした(69f040c・設計 第 10 版 §8-4・検討記録 §27)。check-frozen で f12fc・2a8c とも A1 は 列 一致・行 一致。凍結済みの期待は変えていない。
   - b08e: probe_v2.json(sha16 8e03dede215b1be3)・a2cells_v1(4a2ec56c63bc13b5)・記入票の回 `H1_b08e7fb25c3da376_20261101-1452`(aid3・A2h は H1A2h_20261030-1622 から)を作った。**まだ配っていない**。利用者の一言で 8731 に配り、記入の後に凍結し、info を出す。
   - b08e の検出の結果(目盛の辺・規則性・表の有無)と票の節の数は、凍結の前なので docs・連絡文に書かない(節の数から表の数が分かる)。
5. f12fc の B4(10 と 11)は、利用者が後で確かめる。
7. **b08e 凍結済み**(2026-11-04 01:44): expect_v1 の sha256 は 25b8bd9d542727d6af4b76cab24a77371ad3ab02fec595e64aaf4be5266c03e0。info は `20261104-0145_acc_to_coord_dgr_info_b08e-expect-frozen.md`(74a40af)。設計 第 11 版(§5 の決まり 4・5・§4 の 8)、検討記録 §28。
   - decisions: 不確か 1 節・決まり 2 節。T1 の 1 欄はデモの後に数え直したが、利用者の選択で合否に入れる。受入の報告に「T1 は独立を外した」と併記する。これは decisions.json の by に書いた。C 層の道具では、独立を外した節の欄を設ける。
   - A1 の自己点検は 列 外れ・行 一致。調べは済んだ(2026-11-04・検討記録 §29・754f6fd)。主因は検出の側で、目盛の線が描かれていない区画があり、短い目盛は長さのそろいの規則で外れた。文字の切れはスキャンの範囲による(推定)。記入の誤りを示すものは無い。直しの案(自己点検を、等間隔の並びを延ばした本数で比べる)は利用者に諮る。試しのスクリプトは scratchpad の a1_gap_diag.py。道具は import すると引数を解くので、末尾の前までを exec して使う。
   - 済み(2026-11-04): 利用者が「下書きを消す」を押し、acc が 8731 のサーバーを止めた。entries は 13 行のまま(sha16 d2ee457de60038a8 = 凍結の出所と同じ)。残りは、coord が決まり 2 つの dec を記録すること。
6. 照合(C 層)の道具を作るとき(2026-11-02・dgr の rep 0006 §4・決38): dgr が図面 3 枚の `read_v0.json` を R-G3 第 2 版で作り直した。`enclosures` には R-G5 の囲みが入った。前の版は `mask_v1/_prev/` に残る。版は `read_v0.json` の `run.tool.git_head`(R-G3)と `enclosures.rg5_tool`(R-G5)で確かめる。読むのは凍結の済んだ図(f12fc・2a8c)だけで、b08e は凍結まで読まない。決40(2026-11-02 00:48)で `enclosures` がまた書き直された(`rg5_tool` = 551587b)。これからも書き直されうるので、C 層を作る時点の版を読み、照合の記録に版を書く。b08e の採否の画面(dgr の R-G6)は acc の凍結の後になる。決44・rep 0214(2026-11-04)で、図面 3 枚の kgb_input に XRef(他図参照)と RevisionMark(改訂の印・採否は R-G6 の後)が入った(道具 cfab14d)。read_v0.json は書き直されていない。R-G5 の候補の段の出力は `data/derived/drawing/_vision_rg5/` と図ごとの `vision_rg5/`。B4・B5 の照らしの入力の候補として、C 層の計画で見る。決47(2026-11-04 16:28)で R-G6 の採否が決まった(凍結の後・dgr は期待を読んでいない)。採否の出所は図ごとの `rg6/decisions.jsonl`、写しは `revision_decisions.json`。rep 1634(2026-11-04 16:34)で dgr が kgb_input を書き出し直した(道具 d5afc2c と直し b62ea32)。版の sha16 は rep 1634 の §1-1(revision_decisions.json)と §3(_meta・elements・grid_map)の表にある。前の版は `mask_v1/_prev/rg6_d5afc2c/kgb_input_cfab14d/`。採ったが範囲が未特定の印には gap(insufficient)が付き、struck の要素は無い。C 層では、書き出し直した版(revision_decisions.json の sha16・kgb_input の tool_commit)を記録し、gap の印の B5 での扱いを計画で決める。

**守ること**: info には判断の中身を書かない(数だけ)。これは coord の頼み(2026-10-31)である。

**凍結の info に書き足すこと**(決42 §2・2026-11-02): coord が、すべての資料の実データでローカルのデモを作り、利用者に見せる。b08e と文書系の期待を凍結するときは、凍結の前に利用者に「デモで読取の値を見たのは、記入の前か後か」を尋ねる。答えを凍結の info に書く。後なら、その資料の受入の報告に「独立を外した」と併記する。デモを開く前に b08e の記入を済ませれば、b08e の独立は保てる(b08e を配るときに利用者に伝える)。

関係: [[acc-review-server-shared-with-dcr]]・[[acceptance-independence-no-element-kinds]]

**coord の記録(2026-11-04 01:50)**: info 0145 の頼み(決まり 2 つと T1 の扱い)は決43(`20261104-0150_coord_to_acc_dec_b08e-freeze-counting-rules-and-demo-order.md`・002ba9e)で記録した。設計 第 11 版 §4 の 8 は決42 §2 の具体化として記録し、文書系の凍結にも当てる(推定)。
