# H10-F｜Logbook + Quick Find + Special Lists

## Logbook

Logbook 是历史 Projection；Complete / Uncomplete / Cancel / Delete 是不同 Command，identity 在历史投影中保持。页面不得把 Logbook 当 Trash，也不得把 Delete 实现成 `status=completed`。

## Quick Find

使用 H09-D Overlay，不额外做一个重复的 SearchPage。打开/关闭后恢复来源 view 的 scroll/selection/expanded context。

## Special lists

Tomorrow、Deadlines、Repeating、All Projects、Logged Projects 作为 Query/Navigation destinations。Deadlines 精确页面视觉仍有 A 级缺口，但 Query/type/route 可以先实现；Repeating 必须展示 Series/Copy 语义，不能简单筛 `repeat=true`。

Fixture：completed/uncomplete roundtrip、canceled、logged project、deadline list mock、repeating series/copies、hidden list QuickFind navigation。
