# H10-C｜Upcoming

Upcoming 展示未来承诺/日期型 Projection。页面只消费 `UpcomingQuery → DaySection[]`；Repeat generated copies、未来 Start、Deadline/其他投影来源由 Query contract 决定，页面不复制业务规则。

结构：PageHeader → DaySection\[] → TodoRow/Project presentation。日期本身是 grouping key，不是 parent。Tomorrow 是特殊可寻址列表，但 Upcoming 内 Tomorrow 仍只是日期 section。

Fixture：Tomorrow 2、Day+2 1、跨月、repeat copy、deadline metadata、empty day policy mock、expanded row、selection。Golden：date baseline、section spacing、scroll sticky/non-sticky 行为按证据冻结；未知保持 Pending。

Mutation 后只刷新受影响 DaySections，保持 identity 与 scroll anchor。
