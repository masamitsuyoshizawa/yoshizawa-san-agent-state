---
name: crossing-sounding-readdown
description: 図表KB鳴動条件を決定論読み下しし回答層へ加法注入(停止/終止条件から着手)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-09T12:29:16.665Z
---

図表KB(C)の鳴動条件・制御表の正答化を、鳴動条件テキストの決定論インタプリタ拡張で進行中(利用者承認済・一問ずつ)。基準=`docs/eval/図表KB_208現況再測定_20260808_0525.{jsonl,csv}`(C単体/interlocking/ask・opus-5・全体正答率42.3%)。

**実装済(commit 95970de)**: `kg_api/kb/scripts/sounding_eval.py` に `ordered_tcs/circle_lits/jiso_seconds/derive_stop/read_down_line` を追加。`app.py` の `sounding_readdown_ctx` で `/interlocking/ask` に決定論読み下しを加法注入。発火は「踏切 かつ 停止/終止 かつ 鳴動/警報」の停止条件問のみに限定(諸元・鳴動時分・鳴動条件そのものへは不作用=回帰回避)。鳴動停止の決定論: 終止欄=制御子/秒は原文、終止欄空欄→鳴動条件の最終(最も踏切寄り)在線軌道回路を抜けきると自動終止。

**結果**: 041 correct化。019(52T/H)・017(61ロT)・035 は内容=正答相当だが判定器(sonnet-4-6)は範囲外情報・符号細部で partial 据え置き→利用者判断で「正答相当」扱い(option1)。回帰なし(実効9問=017/018/019/028/035/041/042/051/070、既correct051維持)。鳴動可否問087-094は決定論可否パスで早期returnし不作用。

**トラックD(2026-08-09完了)**: (1)**全522ルールを100%構造化**=`structure_sounding.py` で在線軌道回路/始動点/現示条件(AND-OR)/時素/進路種別/鳴動停止/読み下しを決定論導出し SoundingRule の 開始条件JSON/終止条件JSON/鳴動読み下し へ全件格納(従来67→522・両環境)。全522がparse成功=解釈の構造化は完成。(2)**鳴動可否回答の完全化**=`sounding_answer` に 可否=する→発火進路の鳴動停止条件(derive_stop)、可否=しない→鳴動が始まる条件 を付加。鳴動系 correct 29→31・incorrect 1→0(090回収)、可否correctへの回帰なし。**088-091全correct化(D続き)**: sounding_answerを2点強化=(a)進入進路の秒終止を『行き先番線の軌道回路(=同踏切の「番線-方面」進出進路の先頭TCから決定論導出)進入完了後N秒で鳴動停止』と精緻化(091=1RAT進入後30秒)、(b)可否=しない時のwould-soundを「現在の現示下でTC進入すれば発火する進路」にeval_ruleで絞り「通過進路ではないため61ⅠT進入後に鳴動」と因果明示(088/089)。高宮088/089/090/091を全correct化。**教訓**: 単発208のaggregate correctは±7-10のノイズ(interlockingが変更対象外なのに±9揺れる)でクラスタ改善を覆い隠す。信頼できるのはcrossingクラスタ値(55.3%=最高)。安定集計には多数回平均が必要。016(加速度表)/067(踏切長)はデータ未収録で保留。設計思想/なぜ系(018/028/036/066/070)は収録範囲外の正当な棄権。関連: [[eval-and-table-comprehension]] [[reminder-crossing-control-table-rules]] [[judge-scope-calibration]] [[symbol-legend-2-1a]]。
