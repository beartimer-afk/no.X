# H10-D｜Anytime + Someday

Anytime = active/actionable 且没有未来 Start 约束的 Projection；Someday = 明确 hibernating StartMode，不是 null/date sentinel。二者不能用一个 boolean `scheduled` 互相推导。

同一 Todo 可同时具有 Today membership 与 Anytime 语义条件的重叠情况，具体列表 membership 由 Query 决定；页面不得为了“互斥列表”强行改 Domain。

组合 TodoRow、organization grouping、MagicPlus、Selection、Drag、ExpandedTodo。Someday move back to Anytime 走 Start command，不是移动 parent。

Fixtures：unparented、Area direct todo、Project todo、tags inherited、Today overlap query mock、Someday mixed。Golden 关注分组与留白，不新增 card chrome。
