# H05｜ExpandedTodo

## H05-A｜原地展开容器骨架

ExpandedTodo 不是独立 `TodoDetailPage`。它是同一 TodoRow 在当前列表上下文中的展开 Presentation State。

必须保持对象连续性：

```
Collapsed TodoRow
→ 点击 Row
→ 原位置扩展成 ExpandedTodo
→ 编辑
→ 收起
→ 回到同一列表、同一 Todo、同一滚动上下文
```

首版禁止 `router.push('/todo/:id')`。

## 真机观察

A 级录屏在 Upcoming、Project 等页面确认：

* 展开面仍位于原列表位置；
* 后续 List 内容被向下推；
* Surface 为白色/近白；
* 左上仍是同一个 Todo Checkbox；
* Title 在 Checkbox 右侧；
* 下方有 Notes；
* 底部存在 When / Tags / Checklist / Deadline 等动作区；
* 有轻微圆角与 elevation/shadow；
* 键盘出现后仍保持当前页面上下文。

C 级样本（512×1108）：

* Expanded surface 横向几乎占满列表内容宽度；
* 典型样本纵向约 215 epx，但**绝不能据此固定高度**；
* Checkbox 左缘约 18 epx；
* Title 左缘约 53 epx；
* Notes 与 Title 左对齐；
* Action 区位于 surface 底部约最后 45～55 epx 的区域。

## Slot 架构

```
ExpandedTodo
├─ ExpandedSurface
│  ├─ MainEditorArea
│  │  ├─ CheckboxSlot          [复用 H02]
│  │  └─ EditorContent
│  │     ├─ TitleEditorSlot    [H05-B]
│  │     ├─ NotesEditorSlot    [H05-C]
│  │     └─ ChecklistSlot      [H06]
│  └─ ExpandedActionBar        [H05-D]
│     ├─ WhenSummarySlot
│     └─ ActionIconsSlot
```

H05-A 只实现容器和 Slot，不把所有编辑器一次塞进一个巨型组件。

## Object Continuity Gate

最关键硬约束：

```
checkbox.x(collapsed) == checkbox.x(expanded) ±1vp
title.x(collapsed)    == titleSlot.x(expanded) ±1vp
notesSlot.x            == titleSlot.x ±1vp
```

展开不能让 Checkbox / Title 横向跳动。

## 高度

总高度由内容决定，禁止固定值。Title、Notes、Checklist 增长时 Surface 自适应。

首轮 spacing / radius / shadow 仅为 PROVISIONAL Token，必须使用 Visual Lab + Golden 校准。

## 状态

```
COLLAPSED
EXPANDING
EXPANDED_IDLE
EDITING_TITLE
EDITING_NOTES
EDITING_CHECKLIST
COLLAPSING
```

这些全部是 Presentation State，Domain Todo 不保存 `expanded=true`。

## Stable Key

列表中同一 Todo 使用稳定 `todoId` 作为 key。展开应在同一 item builder 内切换 presentation，避免创建另一份 Todo VM 引发状态闪烁。

## Keyboard

ExpandedTodo 只暴露 ensure-visible / bottom-inset 协作接口；页面或 KeyboardCoordinator 根据 HarmonyOS avoid-area 能力调整滚动位置。

禁止组件内部硬编码某台手机的键盘高度。

## Surface

禁止使用系统 Card 默认视觉、禁止大圆角 Material 卡片感。Things Light Mode 的展开面应是克制、接近白色、只有很弱层级阴影的编辑 Surface。

## 下一子任务

* H05-B TodoTitleEditor
* H05-C NotesEditor
* H05-D ExpandedActionBar
