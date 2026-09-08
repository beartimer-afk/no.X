# H10-B｜Today + This Evening

Today 是工作集 Projection，不是 folder。Membership 由统一 `TodayQuery` 决定，来源包括当前已冻结的 Start/Deadline/repeat 等规则；页面禁止自行写 `when<=today` 这种简化算法。

This Evening 是 Today 内第二级 bucket，由 independent evening flag/presentation 驱动；不是 18:00 时钟，也不是 Reminder。它仍属于 Today，parent 不变。

Group by List 是 presentation preference，不修改 Domain。`Group-by-List × This Evening` 的精确嵌套仍 `PENDING_EVIDENCE`，实现必须通过可替换 SectionBuilder 策略隔离，不能硬编码猜测。

TodoRow、DeadlineMeta、RepeatIndicator、Project grouping、Selection/Batch、Drag、MagicPlus 均复用已冻结组件。

Page fixtures：standard（normal、deadline、repeat、Evening、Project grouped）、selection、expanded、keyboard、empty、grouping-policy mock。Golden 检查 section anchors、Evening divider/heading、row rhythm、bottom controls。
