---
name: pdf-cid-font-recovery
description: ToUnicode欠落CIDフォントPDFはWindows完全版フォントのcmap逆引き(GID→Unicode)で決定論復元できる(連動図表PDFで実証)
metadata: 
  node_type: memory
  type: project
  originSessionId: b32d266c-9a37-4f4c-9a81-54f37d0d5815
  modified: 2026-07-31T10:23:55.700Z
---

JR東日本の一部PDF(例: ある駅の連動図表)はテキスト層があるのに
ToUnicode CMap 欠落+サブセットTTFのcmap除去のため抽出文字が chr(GID) に化ける
(pdfplumberでは (cid:9887) 等)。元フォントが Windows 標準(MS 明朝/MS ゴシック/游ゴシック)
なら、WSL の /mnt/c/Windows/Fonts/*.ttc を fontTools で開き cmap を逆引き
(GID→Unicode)すれば全文字を決定論復元できる(未復元0文字で検証)。
実装: `scripts/t2_pdf_det_table.py`(2026-07-31)。ページ rotation=90 格納にも注意
(page.rotation_matrix で座標変換)。関連: 一部の版のPDFは連動表本体が
ラスタ画像でテキスト層なし。詳細記録は 2026-09-24 の顧客データ削除で削除済み。

**Why:** PDFのみ提供の駅への拡張時、ビジョン転記(確率的・3パス多数決が必要)より
先にこの決定論経路を試すべきため。
**How to apply:** pdfplumber で (cid:N) が出たら埋め込みフォント名を確認し、
Windows 標準フォント由来なら cmap 逆引きを適用する。
