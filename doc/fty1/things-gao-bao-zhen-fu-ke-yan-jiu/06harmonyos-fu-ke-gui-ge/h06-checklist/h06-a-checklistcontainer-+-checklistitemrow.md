# H06-A｜ChecklistContainer + ChecklistItemRow

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 产品事实

Things 官方明确：Checklist 用来拆解 Todo 的较细步骤；Item 左侧圆圈点击完成，右侧三横 grip 长按拖动排序，左滑删除。Checklist 顺序只在所属 Todo 内生效。

## Domain 边界

```
ChecklistItem {
  id
  todoId
  title
  status
  rank
}
```

ChecklistItem 不是 Todo，不具备 Project/Area/Heading/Start/Deadline/Tags/Repeat。

Notes 中 `- [ ] text` 是 Markdown 文本，绝不能自动创建 ChecklistItem。

## Structure

```
ChecklistContainer
├─ ChecklistItemRow[0]
├─ ChecklistItemRow[1]
└─ NewItemSlot [H06-B]

ChecklistItemRow
├─ CompletionControl
├─ Title
└─ DragGrip
```

## Events

`onToggleChecklistItem`、`onRequestEditChecklistItem`、`onDeleteChecklistItem`、`onBeginChecklistDrag`、`onMoveChecklistItem`、`onEndChecklistDrag`。组件只发 intent/command，不直接写 Repository。

## Layout

Checklist 位于 ExpandedTodo 的 EditorContent 中，和 Notes/Title 同一内容列，不创建独立 Card。长标题自然换行，Grip 固定右侧，不能被标题挤出屏幕。

## CompletionControl

Checklist completion circle 与 TodoCheckbox 不是同一最终组件。可以复用 circle/checkmark primitive，但必须有 Checklist 专用 size/color/hit-target Token。当前精确视觉缺 A 级近景，因此保持 PROVISIONAL/PENDING。

点击完成 ChecklistItem 不能完成父 Todo，也不能改变父 Todo 的 Start/Deadline/membership。

## Completed Visual

当前精确删除线、灰度、circle fill、是否自动下沉等均保持 Pending，禁止直接套 Todo completed 视觉。

## DragGrip

必须是项目自有三横 grip vector。只允许 grip 长按启动 drag，整行长按不能启动，避免与 title 编辑/选择冲突。

## Reorder

排序只改变同一 todoId 内 ChecklistItem.rank。禁止拖出 Checklist、拖到另一个 Todo 或复用 Todo 全局 sortIndex。

## Swipe Delete

左滑只删除 ChecklistItem，不是 Cancel。精确距离/颜色/动画待 A 级补证；功能可先实现，视觉 PROVISIONAL。

## Gesture Priority

Completion tap → Title edit → Grip long-press drag → Row horizontal swipe → Parent vertical scroll。

Checklist 手势必须与父 Todo selection/drag/delete/collapse 隔离。

## Stable IDs

`checklist.root`、`checklist.item_<id>.root`、`.completion`、`.title`、`.grip`、`.swipe_delete`。

## Fixtures

C01 empty；C02 one\_open；C03 three\_open；C04 one\_completed；C05 mixed\_status；C06/C07 long title；C08 pressed completion；C09 grip pressed；C10/C11 swipe；C12/C13 drag；C14 narrow；C15 disabled。

Completed 相关 Fixture 在无 A 级视觉证据时只能 `VISUAL_PENDING`。

## Geometry Gate

completion/title/grip 互不覆盖；Grip 不越界；长标题不遮 Grip；Row width 等于 ChecklistContainer content width；hit targets 不重叠。

## Interaction Gate

Completion 只触发 checklist toggle；Title 只请求编辑；Grip drag 只产生 local reorder；Swipe 只揭示 checklist delete；Parent Todo callbacks 全为 0。

## Paste 边界

官方 iPhone 支持在 Checklist field 粘贴多行，每行创建一个 Item；单次最多 100 行是防误操作限制，不等于 Checklist 总容量。具体实现放 H06-B。

## Search 边界

Checklist 内容进入 Quick Find Continue Search 的深度索引，但 iPhone Find in Text 不搜索 Checklist，因此不能把 Checklist 文本并入 NotesEditor。

## Pending

* PENDING-CHECK-01 Typography
* PENDING-CHECK-02 Swipe 视觉/距离/动画
* PENDING-CHECK-03 Completion circle 精确视觉
* PENDING-CHECK-04 completed title
* PENDING-CHECK-05 completed item 排序
* PENDING-CHECK-06 drag preview/insertion/haptic
* PENDING-CHECK-07 canceled UI
* PENDING-CHECK-08 row minHeight/gap

## 低能力模型顺序

Container/open row → Completion skeleton → title/long title → fixed Grip → event isolation → swipe functional → drag intents → local rank command → ExpandedTodo composite → open-state Golden → LOCK。

## 禁止事项

ChecklistItem 不得当 Todo；不得与 Notes task list 互转；不得全完成自动完成父 Todo；不得整行长按 drag；不得 Unicode grip；不得 Swipe Delete 映射 Cancel；不得用 Todo 全局排序；不得让父 Todo 手势泄漏。

## VERIFIED

Functional：open/long title Geometry、completion isolation、grip-only drag、local reorder、swipe delete、stable ID、ExpandedTodo composite、H05 regression PASS。

Visual：open-state 可先冻结；completed/swipe/drag 精细视觉必须等 A 级近景补证后升级。

下一子任务：**H06-B｜ChecklistItemEditor / Create / Paste**。
