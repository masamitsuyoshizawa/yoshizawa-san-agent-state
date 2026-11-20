---
name: hierarchy-contract-v07-field-names
description: 階層の作り直し(決168)の入力の契約 v0.7 の欄の名(kgb・2026-11-20 承認)・acc の期待の第 2 版で同じ名を使う
metadata:
  type: project
---

kgb が、決168 の段 1(入力の契約 v0.7)の欄の名を、kgb の画面の Plan Mode で利用者に諮り、承認された(2026-11-20・kgb のメッセージ)。acc の階層の期待の第 2 版は、同じ名で作る。

- 種類 `block`: heading(見出し)・item(箇条)・body(本文)・table_figure(表と図)。Section と Point は dcr が heading か item を書く。Chunk は body、Table と Figure は table_figure(kgb が補う)。グラフの属性は a_block。
- 原本の印 `marker_raw`: 行頭の印を原本の字のまま(印の無い行は持たない)。グラフは a_marker_raw。
- 親の決まり方 `parent_basis`: rule・human・provisional(決まらず、すぐ上の行の内訳として仮に置いた)。グラフは a_parent_basis。欠けの節点は立てない。
- 深さ `depth`: kgb が親の鎖から数える。文書 = 0。作業標準は、手順(WorkProcedure)= 0・節 = 1・項は 2 から(入れ子はさらに 1 ずつ)。グラフは a_depth。
- 親: 今の辺のまま(節は PART_OF・塊と表と図は IN_SECTION・作業標準の項は節から HAS_POINT)。作業標準の項の入れ子だけ、項 → 親の項の PART_OF を足す。
- 見出しの種類の語 `kind`: 附則・別表などは null でよい(block が heading で marker_raw が在る時だけ)。新しい語は作らない。

**Why:** 決168 の段 2 で、acc は、凍結済みの 4 本の期待の第 2 版(種類・深さ・親を別々に照らす)を作る。欄の名が食い違うと、照らしの道具で読み替えが要る。

**How to apply:** 階層の計画(acc の画面の Plan Mode・記入票の道具の後)で、期待の第 2 版の欄を、この名で書く。acc の深さは「章の行 = 1」で数えていたので、kgb の数え方(文書 = 0)に合わせるか、親で比べる(今までどおり)。確かめ直しの第 2 版(決169 の部 C)と、階層の第 2 版は、同じ本に当たるので、版の上げ方を 1 つにまとめる(rep 20261120-1233 §4)。dcr との合わせで欄が変わる時は、kgb がまた知らせる。関連: [[acc-qa-eval-audit-plan]]
