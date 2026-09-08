# H10-A｜Main Lists + Inbox

## Main Lists

它是全局导航与组织总览，不是一个 Todo parent。组合 SystemListRow、AreaRow、ProjectRow、MagicPlus、QuickFind entry。Area 中可含 Project 与 direct Todo；Main Lists 的精确 direct Todo 展示策略若无证据则由对应 Query/页面研究约束，不得凭感觉新增。

## Inbox

Inbox 表示 capture/unclarified 的组织状态/投影，不应通过“projectId=null 就一定是 Inbox”这种简化推断覆盖 canonical rule。TodoRow 原地展开；MagicPlus 创建默认进入当前 Inbox context。

## State

Selection、Drag、ExpandedTodo、Overlay 都由全局 controllers 协调。页面保存 scroll/expanded anchor；跨 view mutation 后通过 Query refresh 更新，不重建实体。

## Fixture / Golden

Main Lists：system lists + 2 Areas + projects + collapsed area。Inbox：empty、3 normal、long title、expanded、selection、keyboard。检查 safe area、header baseline、row rhythm、MagicPlus bottom position、scroll restore。
