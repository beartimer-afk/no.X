# H06-C｜Drag Reorder / Swipe Motion / Final Regression

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

## 目标

完成 Checklist 的 Grip 长按拖拽、Drag Preview、target gap、mixed-height reorder、drop/cancel、local rank commit、auto-scroll hook、Swipe Reveal/Delete、手势互斥以及 H06 最终回归。

## 平台结构

不为 Checklist 再嵌套独立滚动 List。推荐普通 Column/Layout + `ChecklistDragCoordinator` + `SwipeRevealRow`，外层 Page/List 继续负责纵向滚动。

## Drag 激活

只有 DragGrip 可启动 reorder：LongPress → armed → Pan → dragging。Title/Completion/整行 long press 不得触发 Drag。HarmonyOS 500ms 等平台默认只作为 Seed，Things 精确阈值保持 PROVISIONAL。

## State Machine

`IDLE → GRIP_PRESSED → DRAG_ARMED → DRAGGING → DROPPING → IDLE`；取消走 `CANCELING` 并恢复原顺序。所有 drag state 都是 Presentation，不写 Domain。

## Preview / Placeholder

Preview 保持 ChecklistItem 对象连续性，原位置维持等高 placeholder。Scale / shadow / opacity、source placeholder 形态、haptic 均等 A 级补证。

## Mixed-height Target Index

ChecklistItem 可多行，必须用真实 item rect / midpoint 算 target index，禁止 `floor(y/fixedHeight)`。Fixture 必须混合 1/2/3 行高度。

## Presentation Reorder 与 Domain Commit

拖动过程中只更新 Presentation order；Drop 时一次 `reorderChecklistItem(todoId,itemId,before?/after?)`，不得每帧写 DB。itemId 不变。

## Sibling Reflow / Auto-scroll

相邻 item 应平滑让位；duration/curve 保持 Pending。靠近 viewport 边缘时只通过 `onRequestAutoScroll(direction,intensity)` 请求外层 Page/List 滚动，Checklist 不自建 ScrollController。

## Cancel / Boundary

系统取消、页面失焦、数据冲突等必须恢复原 Presentation 且不写 rank。ChecklistItem 只能在所属 todoId 内 reorder，Drop 到 Notes/Title/ActionBar/别的 Todo 都不能改 parent。

## SwipeRevealRow

Foreground Row 监听向左水平 Pan，Background 是 Delete Action。支持 partial → snapback、reveal → tap delete。无 A 级证据前不默认开启 full-swipe auto delete。

Checklist swipe 必须阻断父 Todo multi-select/swipe；Drag 与 Swipe 不允许同时 active；编辑态 Swipe/Grip 采用 Policy 控制。

## Motion / Undo

Delete 后 row collapse/reflow 等精细 motion 和是否存在短暂 Undo 仍 Pending；UI 不自行发明 Snackbar/Toast。

## Stable IDs

`checklist.drag.preview`、`.placeholder`、`.target_gap`、`checklist.item_<id>.swipe.root/background/delete`。

## Fixtures

Drag：上下移动、mixed height、first↔last、cancel、edge auto-scroll、editing/completed。

Swipe：partial snapback、reveal、delete、second closes first、parent isolation、editing、long title、completed。

## Motion Script / Golden

记录 drag activated / moving / target gap / drop / settled，以及 swipe 25% / threshold / reveal / delete / settled 的 deterministic 帧。初版作为 HarmonyOS internal regression golden；无 A 级近景前不得宣称 Things pixel-verified。

## Geometry Gate

placeholder.height == source original ±1vp；preview width 与 source 一致；mixed-height target 正确；Swipe 只水平位移且 row height 不变；Parent Todo root 不位移。

## Performance

50+ item reorder 时 pointer update 不重建整个 Todo/Page；每帧不写 DB；拖拽时不重跑 Markdown parser。

## Final Regression

覆盖 Structure、Completion、Edit、Create/Paste、Drag、Swipe、Identity、Parent isolation、ExpandedTodo composite、Keyboard ensureVisible。

## Pending

* CHECKDRAG-01 Source placeholder
* CHECKDRAG-02 Haptic
* CHECKDRAG-03 Auto-scroll threshold/speed
* CHECKDRAG-04 editing grip
* CHECKDRAG-05 Preview scale/shadow
* CHECKDRAG-06 reflow motion
* CHECKSWIPE-01 reveal width/color/icon/full-swipe
* CHECKSWIPE-02 editing swipe
* CHECKSWIPE-03 delete motion
* CHECKSWIPE-04 Undo

## 低能力模型顺序

state machine → grip long-press+pan → mixed-height geometry → target/placeholder → Presentation reorder → one-shot drop command → cancel → auto-scroll hook → SwipeRevealRow → mutual exclusion → parent isolation → 50-item performance → final regression → internal Golden。

## 禁止事项

不得整行 drag、固定 row 高度算 index、每帧写 DB、跨 Todo drop、为 swipe/reorder 嵌套滚动 List、Drag+Swipe 同时 active、无证据开启 full swipe、平台示例参数冒充 Things Token。

## H06 完成定义

H06 Core = `FUNCTIONAL_VERIFIED`：Completion/Edit/Create/Paste/Reorder/Delete、stable identity、parent isolation、keyboard/focus continuity、ExpandedTodo composite、H05 regression 全 PASS。

H06 Visual = `PARTIAL_VERIFIED`：open/base editor 可冻结；completed/drag/swipe 精细视觉等 A 级近景后升级。

下一主任务：**H07｜Organization Components：ProjectRow / AreaRow / HeadingBlock**。
