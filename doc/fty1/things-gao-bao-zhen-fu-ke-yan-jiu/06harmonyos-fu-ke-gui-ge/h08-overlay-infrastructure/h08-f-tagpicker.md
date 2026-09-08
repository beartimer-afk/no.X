# H08-F｜TagPicker

TagPicker 编辑 **direct tags**。Area / Project 继承产生的 inherited/effective tags 只能作为上下文提示，禁止复制到 Todo.directTags。

## Domain

`Tag` 独立实体；Todo/Project/Area 与 Tag 为多对多关系。Presenter 输出 `directTags / inheritedTags / effectiveTags`。Picker 的可编辑选中态只对应 directTags。

## UI composition

复用 H08-A/B。结构：SearchField、Selected/Direct 状态、Tag rows、可选 CreateTag intent。Inherited 标签如果展示，必须明确为只读来源，不得伪装成直接选中。

## Commands

Single：`SetDirectTags` 或更细的 `AddDirectTag/RemoveDirectTag`；Batch 必须显式处理 `ALL_SELECTED / NONE_SELECTED / MIXED`，不得读取第一项状态冒充整个 selection。确切 batch UI 呈现可用 mixed indicator，视觉待 Golden。

## Search

只搜索 Tag registry，不等同 Quick Find。大小写/中文匹配策略由 TagQuery 统一提供，Picker 不自写第二套搜索算法。

## Fixtures

none、single、multiple、inherited-only、direct+inherited same effective result、long tag name、search、batch mixed、empty registry、create-tag intent。

## Acceptance

Tag identity 不变；direct/inherited 不互相污染；batch command 原子提交；关闭后 ExpandedTodo/Row metadata 重新由 Presenter 计算；accessibility 与 keyboard/focus restore PASS。精确 row height、checkmark、tag typography 继续 Golden 校准。
