---
name: agent-env-repo-kickoff
description: 外部エージェント(別組織・別 EC2)の環境分析のため別リポジトリ ~/jrtokai-agent-env を 2026-10-28 に新設(決326)・写し + 目録・分析セッション 1 つ・intake/ は利用者が置く
metadata:
  type: project
---

2026-10-28 に利用者の依頼(決326)で、**本 PJ の API を使う外部エージェント**(別の方が別途作成し別の EC2 で稼働・本 PJ の Dify/Streamlit デモは対象外)の環境を分析・評価し改変の準備をする場として `~/jrtokai-agent-env`(新規 git・main・初回コミット・GitHub origin は利用者が後で)を作った。

- 置いたもの: CLAUDE.md(隔離規則・単一セッション・本 PJ は読み取り専用参照)・docs/00 目的と範囲・01 提供資料目録(ref/ 11 ファイルの sha16 と本 PJ main の版)・02 受入チェックリスト(欲しい資料 10・漏洩点検・目録の様式)・03 評価の観点(API の使い方 A1〜A7・設計 B1〜B5・性能 C1〜C3・改変の様式)・04 起動プロンプト・ref/(chmod 444)・intake/(git 外)・work/。
- 外部エージェントのコード・設定は**利用者が後日 intake/ に置く**。分析セッション(Claude Code)は新リポジトリで単独運用し、連絡箱は使わない。本 PJ の API 変更が要る提案は本 PJ の dec に戻す。

**How to apply:** 新リポジトリの作業はそのセッションで。本 PJ 側で API/契約が変わったら ref/ の写しと目録の版を更新する(coord)。関連 [[accident-vector-similar-item]]・[[diagram-dynfw-phase0-kickoff]]。
