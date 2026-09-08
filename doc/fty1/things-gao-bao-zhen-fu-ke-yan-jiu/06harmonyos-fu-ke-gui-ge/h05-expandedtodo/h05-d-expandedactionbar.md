# H05-D｜ExpandedActionBar

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

ExpandedActionBar 是 ExpandedTodo 底部的轻量操作区，不是通用 Toolbar，也不是独立页面导航栏。它负责把 When、Tags、Checklist、Deadline 等高频属性入口集中在展开 Todo 的底部，同时保持编辑正文区域尽可能干净。

## 结构

```
ExpandedActionBar
├─ WhenSummarySlot            [左侧，可压缩]
└─ ActionIconsSlot            [右侧，不被标题/When 挤出]
   ├─ Tags
   ├─ Checklist
   └─ Deadline
```

当前真机证据显示左侧 When/日期信息与 Title 内容列保持视觉关联；右侧是一组低权重线性图标。首版禁止使用系统 BottomNavigation、Toolbar 或 Material 风格按钮。

## 事件边界

ActionBar 只发 Presentation Intent：`OpenWhenPicker` / `OpenTagPicker` / `OpenChecklist` / `OpenDeadlinePicker`。组件内部不得直接修改 Todo Domain。

WhenSummary 只显示 Presenter 已计算好的语义；它自己不判断 Today / Tomorrow / This Evening。

Tags 入口必须区分 directTags 与 inherited/effectiveTags：编辑只修改 Todo direct tags，不能把 inherited tags 复制回 Todo。

Checklist 按钮只负责切换/聚焦 H06 Checklist 区域；Checklist 数据由独立 Domain Entity 维护。

## 布局合同

* ActionBar 宽度 = ExpandedTodo content width；
* 左侧 WhenSummary 可缩短/ellipsis；
* 右侧 actions 必须保持最小 hit target；
* action icons 在任何 fixture 下不得被 When 文案推到屏幕外；
* ActionBar 不设置固定屏幕坐标，由 ExpandedTodo 容器决定底部位置；
* visual icon size 与 hit target 分离。

精确 icon size、gap、bottom padding、separator、pressed state 当前继续走 Token + Golden 校准。

## Stable IDs

`expanded_action_bar.root/when/tags/checklist/deadline`。

## Fixtures

无 When、Today、Tomorrow、This Evening、long date、Tags active、Checklist active、Deadline active、三项同时 active、narrow width、keyboard visible。

## 验收

Structure / Geometry / Event isolation / keyboard continuity / H05 composite regression 必须 PASS。精确图标、颜色、阴影与动效无 A 级近景时保持 `CALIBRATION_REQUIRED`。

## 禁止事项

不得把 ActionBar 做成导航栏；不得在组件内计算日期；不得把 inherited tags 写入 directTags；不得系统默认 Toolbar；不得因为右侧空间不足隐藏关键操作；不得 ActionBar 自己写数据库。
