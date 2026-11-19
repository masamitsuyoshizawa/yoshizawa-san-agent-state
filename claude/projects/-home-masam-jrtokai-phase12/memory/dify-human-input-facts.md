---
name: dify-human-input-facts
description: Dify 1.16.1 の human-input 節点(ボタン回答)で EC2 実測した事実と落とし穴(決289〜290・2026-10-22)
metadata:
  node_type: memory
  type: project
  originSessionId: 33062a8e-fea8-4862-bc8f-2385b718c852
  modified: 2026-10-22T12:53:39.681Z
---

EC2 の Dify 1.16.1 Community で、探索木のボタン回答(決289 設計・決290 実装着手)のために実測した事実。

- **描画**: カードの form_content は Markdown で、**mermaid の図も描ける**(定数でも変数差し込みでも)。カードは同じ吹き出しの上に順に積まれ、answer 節点の本文は下に**区切りなく連結**される(各段の answer の先頭に `\n\n`)。
- **構造**: ループの中に置けず循環も不可 → N 段を直列に生成。ボタンは固定(本文だけ動的)。押したボタンの id が辺の sourceHandle。期限切れの辺は実行側の名 **`__timeout__`**(エディタは `__timeout`)。DSL に直接書けば 1 時間後に実際に通る(実測)。`delivery_methods[].id` は UUID 必須(違うと「Workflow stream ended without a terminal event」で止まる)。
- **止まっている間**: Web アプリの入力欄は使える。送ると同じ会話に paused の実行が 2 つになる。再開した実行は止めた時点の変数の写しを使い会話変数を読み直さない → 守りは **G1**(押した後に http-request でサービス API `GET /v1/conversations/{c_id}/variables?user=…` から run_token を読み直す)。古いカードを押すとボタンは灰色になるが再開後の回答は画面に出ない(DB には残る)。押し直しは 412(通知の文言は「another user」)。
- **G1 の落とし穴**: `http://api:5001` は SSRF 保護で遮断。`http://172.31.4.27:8081`(Dify の nginx・内部 IP)は通る(EC2 の Dify に `SSRF_PROXY_ALLOW_PRIVATE_IPS=172.31.4.27/32` が設定済み)。`variable_name=` の絞り込みは 500(json_extract_path_text(text) の不具合)→ 一覧を取り code で選ぶ。鍵はアプリの環境変数 secret。
- **道具の注意**: EC2 で `python3 -I` を使うと PYTHONPATH が無視され sitecustomize(DIFY_LOCAL の名前解決の差替え)が効かず console のログインが時間切れになる。一時アプリは `tmp_` を付け測後に削除。写しと道具 = `knowledge_kb_v8/data/dify/human_input_spike_20261022/`(hi_make*.py・hi_probe*.py)。

**Why:** 設計(fed)がソースの静的読みで置いた前提のうち 2 つ(入力欄が使えない・再開後の出力の並び)が実機と違った。実機で確かめてから設計を確定する。
**How to apply:** human-input を使う DSL を作るときは上の事実を前提にし、変えるときは一時アプリで測り直す。関連 [[ec2-dify-dsl-lineage]]・[[tree-redesign-candidates-history]]。
- **決294〜296(2026-10-23)**: 「わからない」→「別の質問にする」(id other)。押しても tool を呼ばず同じ図の並びで次の未回答をカードに(図なし)・差し替え済みはこの実行の中だけ持ち越し・上限 2/段・asked に足さない。3 集約は新しい順(差し替え 2 → 1 → 初回)。公開 URL のトークンは publish で変わることがある(d_url を取り直す)。


(索引から移した説明・2026-11-07)
- [Dify human-input の実測事実](dify-human-input-facts.md) / [決290 ボタン回答の受入(bkb)](dify-button-acceptance-r290-bkb.md) — カード内 mermaid 可・受入は決293・決295 差分合格
