---
name: v8-cross-source-federation
description: V8 で A(基準RAG)/B(事故V7)/C(図表V6) の横断統合が完成・出典検証済み
metadata: 
  node_type: memory
  type: project
  originSessionId: 7f36198d-cf22-40c9-8ff2-bf20bd49be0b
---

knowledge_kb_v8 に A/B/C 横断統合フェデレーションを構築済み(2026-06-08 時点)。

- A=基準・手順RAG(BGE-M3+BM25→RRF→reranker, 索引2977チャンク), B=事故摘録V7(Neo4j bolt 9890, 全文+決定論診断 accident_diag_engine), C=連動図表V6(Neo4j bolt 9790・凍結/読取専用, 短縮ID接地+NL→Cypher)。
- 中核: `knowledge_kb_v8/scripts/federation.py`(federate(): 単独/統合を選択, audit_citations() で決定論の出典監査), CLI `federate.py`, 評価 `eval_federation.py`, UI は `demo/app.py`(8508)の「横断統合モード」トグル。
- 検証: 層化12事象で統合回答の事故ID裏付け1.00(151件)・文献出典1.00(59件)。A単独の出典帰属は0.95。
- 規約遵守: 全Cypher読取専用, V6凍結, B はマスク済サマリのみ。data/index は git 外。評価は [[prefers-stratified-sampling]] に従い現象クラスで層化。

未着手候補: C の NL→Cypher 成功率評価 / 横断統合の AWS 配備。
