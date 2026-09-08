# H08-G｜MovePicker

MovePicker 只表达组织结构迁移，不改变 Start / Deadline / Tags / Repeat。

## Target model

Todo 的合法组织父级：Project / Area / none（Inbox/无父级语义由 Command 定义）。Heading 只能在 Todo 已属某 Project 时作为该 Project 内 placement；Heading 不是任意全局父级。Project 可属于 Area 或 none；Area 可直接含 Todo 和 Project。

## Query

Picker 不自己拼树。`MoveTargetQuery(subjectType, subjectIds)` 返回合法 target tree + disabled reasons。禁止 UI 用当前页面节点递归猜合法目标。

## Commands

Todo：`MoveTodo(todoIds,targetParent,targetHeading?,position)`；Project：`MoveProject(projectIds,targetArea?,position)`；Heading 的跨 Project Move 使用 H07 已冻结的结构迁移 Command，并携带 child Todos。所有 Move 必须保持 identity，不能 delete+recreate。

## UI

复用 H08 Panel primitives；支持层级缩进、current target、disabled target、search/filter（如最终需要）。Exact disclosure/collapse、row icons、indent、selection check 由 Golden 校准。

## Regression

Move 后必须重跑 Main Lists、Area、Project、Inbox/Anytime/Today 等相关 Projection；时间字段不变；manual ContextOrder 仅更新受影响 context。

## Fixtures

Todo from Inbox→Area、Inbox→Project、Project→Area、Project→none、Todo Project A→Project B、same-project heading placement、illegal self/descendant target、batch mixed parents、Heading group move delegation。

## Acceptance

合法目标过滤、原子 Command、identity 保持、order 更新、cancel no-op、projection regression、accessibility PASS。
