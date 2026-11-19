---
name: auth-boundary-values-notify-before-change
description: 承認済みの設計の認証の境界(§8)に書いた値は、実装で変える必要が出たら変える前に知らせる(理由が正しくても)
metadata:
  type: feedback
---

設計で固定した認証の境界の値(review_kit の §8・Referrer-Policy など)を、実装の中で「理由が明らかだから」と替えてから報告した(2026-11-14・no-referrer → same-origin・chromium で form の Origin が null になり方法 B が通らなかった)。coord は「決357 追補 7 の『認証の条件を変える前に rev の確認』に当たる」とした(今回は戻さず、rev の点検の依頼の先頭に書き、設計を先に直して版を上げる扱い)。

**Why:** 承認済みの設計の境界の値は、利用者の承認と rev の確認の対象。実装者が理由つきでも黙って替えると、点検の対象の設計と実装がずれる。

**How to apply:** 実装中に §8(認証)・安全側・指紋の順など「変える前に rev」と決めた所の値を替える必要が出たら、替える前に止めて coord に知らせる(材料 = 理由・実測・影響の範囲・否定例の試験)。替えた後は、rev の点検の前に設計の文書を直して版を上げる。関連 [[review-screen-redesign-fed]]・[[approval-line-for-own-artifacts]]
