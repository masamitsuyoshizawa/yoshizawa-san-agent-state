---
name: dict-prototype-3layer
description: 3層辞書プロトタイプ完成(設備/故障様式/表示)・顧客レビュー待ち・候補語彙側への反映が次段
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-27T02:12:11.453Z
---

3層辞書プロトタイプ(2026-08-26構築・コミットdcaf40e・EC2反映済 image dict-20260828):
- 正本: kg_api/kb/config/dict_{equipment,failure_modes,display_codes}.json(status=confirmed/proposed区別保持)
- 規模: 設備1,602エントリ・別名1,226 / 故障様式surface_map 898+新述語候補148 / 表示63
- カバレッジ: B設備語彙2,107種の63.6%(決定論33.3%+LLM proposed)・故障様式86.8%
- 構築: knowledge_kb_v8/scripts/dict/(build→propose→apply、決定論+LLM判定案・span検証)
- 顧客レビューCSV3本: knowledge_kb_v8/data/dict/(git外)。③表示辞書は故障コード表未受領のため逆引き構築
- DIRECT_CAUSE_TERMSはdict_failure_modes.json environmentalが正本(dkg_backendが読込・内蔵はフォールバック)
- LOO700 v5=入力照合接続は実質ニュートラル(correct 16.2%)。
- 台帳代替強化(2026-08-27・コミットa7ac155・EC2 image dict-20260829): 索引語彙LLM選別1,358entry+
  B共起親推定(confirmed51)+B文脈(masked)再判定603+略語展開(特発=特殊信号発光機)で
  **3,563entry・B語彙カバレッジ89.7%・gold文カバー73.8%**。unknown残58。
- **設備軸候補提示dict_eq**(承認済スコア規則 score=0.8+min(0.4,b_freq/20)): 入力設備語→②辞書
  surface_mapのB実績故障様式を候補提示。**LOO700 v6=correct 21.5%/incorrect 48.5%(初の5割切り)**
- 品質是正済(v7/v9): 空cause534件=HB検査項目に原因列なし→FMEA modeが反転生成で捨てられていたのが
  原因。mode突合で全補完(fix_empty_causes.py・cause_source='fmea_mode')・グラフ置換(空Cause99→0・
  Cause1,404)・mode候補×0.7減衰(承認済)。LOO700 v9=correct 19.9%/correct+partial 46.4%(最良)・
  correct単独はv7(21.2%)が上=網羅性とのトレードオフを利用者承認で採用。levelゆれ28件も是正済
- 残課題: JIS用語取り込みは顧客承認後。EC2 D-KGグラフ同期はdump方式(イメージはコンテナと同一版を使う)・
  EC2の/app/kg_api/kb/data/dkbはホスト/home/ubuntu/jrtokai-v9abc/dkbのROマウント(更新はホスト側へcp)

**Why:** vocab_gapの核心は正解側語彙。入力照合(v5)でなく候補生成側に辞書を効かせるdict_eqが+5.3ptの主因。
**How to apply:** 顧客レビュー返却後: レビューCSVのconfirm=y分を確定→グラフ加法投入→再測定。関連 [[dkg-cause-kg]] [[eval-and-table-comprehension]]
