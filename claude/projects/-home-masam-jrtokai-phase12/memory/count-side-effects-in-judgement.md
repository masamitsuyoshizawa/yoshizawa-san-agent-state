---
name: count-side-effects-in-judgement
description: 出力に記録されていた件数(logger の回数など)を判定に入れずに見逃した(rep 1322 で木の logger 7 回 → rev R230-1)・証跡に在る量は件数の期待まで書く
metadata:
  type: feedback
---

2026-10-10 決230 の受入(rep 1322)で、木の要求の口の logger が 7 回(根と枝で重ねて)出ていたのが証跡に残っていたのに、「発火したか」だけを見て件数を判定に入れず、rev に R230-1 として指摘された。回し直し(rep 1429)で「要求につき 1 回・接頭辞つきは 0」を判定に入れ、前の版の 0/7/7 を陽性の対照にした。

**Why:** 「出る/出ない」の二値で見ると、重複や欠け(観測だけのとき 0)が合格に紛れる。
**How to apply:** 記録・印・行のように数えられるものは、期待を件数で書く(要求につき何回か・どの札か)。証跡に出た値を読み流さず、期待の件数と照らす。関連 [[claim-wider-than-implementation]] [[check-both-branches-of-a-condition]]
