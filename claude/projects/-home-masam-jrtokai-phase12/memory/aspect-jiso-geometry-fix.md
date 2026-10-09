---
name: aspect-jiso-geometry-fix
description: 現示種類の斜線独立判定と時素の左右差判定(JR東海Q2-1/Q2-2回答で確定)
metadata: 
  node_type: memory
  type: project
  originSessionId: a8683e03-1306-4a31-bc43-3323a207949a
  modified: 2026-08-03T15:00:45.134Z
---

踏切制御図の信号機の現示種類・時素の幾何抽出(`kb_demo_v6/crossing_jiso_geom.py detect_aspects`)を2026-08-03に是正(JR東海回答反映)。

**現示種類(Q2-1)**: 円内線→現示。正立基準で 3-9時横=警戒 / 12-6時縦=減速 / 1.5-7.5時斜=注意 / 4.5-10.5時斜=進行。検出フレーム(90°回転)では line0=減速, line90=警戒, **line45=注意, line135=進行(mast非依存、全94図統計で確認)**。旧実装は斜線を「×=進行+注意」一括判定し、片斜線のみで誤って「進行」を出し「注意」を落としていた(井田川~亀山下り1が誤読)。斜線独立判定に是正。

**時素(Q2-2)**: 円下部マスト側の黒塗り。マスト側±55°暗率(frac)と反対側(frac_opp)の**左右差diff≥0.08**で判定。黒塗りは偏在=非対称、内部線は中心対称でdiffに効かない(密度に頑健)。旧実装は絶対値frac≥0.30で淡い黒塗り(河曲12R/2L)を取りこぼし。全図diff分布は二峰性(0.00対称/0.10非対称)、谷0.08。時素True 76→174件。

**再適用**: `fix_aspects_only.py`(現示種類・時素のみSET、方向/種別/所属駅は非変更=影響最小・非破壊)→9990適用済。井田川下り1=減速・注意・警戒・停止、河曲12R/2L=時素True に是正。interlocking-013/027/034 の事実部分が正答化(理由・従属関係の未収録部でpartialは正当)。

**要フォロー**: 同一論理信号が複数踏切図に描かれ現示/時素が食い違う多重描画の不整合あり(下り1が5描画中2正/3旧)。APIは概ね正しい描画を拾うが、richest現示等での reconcile が残課題。EC2 cgraph反映も未(全フェーズ後にまとめて)。[[eval-and-table-comprehension]] [[signal-symbol-recognition]] [[judge-scope-calibration]]
