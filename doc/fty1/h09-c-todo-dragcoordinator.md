# H09-C｜Todo DragCoordinator

Todo Drag 负责 presentation drag state 与 `DragIntent → Domain Command`，不得在手指移动时持续写数据库。

## State

Idle → Detecting → Dragging → HoverTarget → Dropping/Cancelled → Idle。Preview、insertion indicator、auto-scroll 都是 UI transient state。

## Gesture

HarmonyOS 当前官方资料确认 LongPressGesture 与 PanGesture 可通过 GestureGroup 组合实现长按后拖动；系统 drag 与自定义 long press 存在冲突时需显式处理手势优先级。具体 duration 不直接当 Things Token，真机证据优先。

## Drop intents

同 context reorder → `ReorderInContext`；跨 Project/Area/Heading → Move command；Today/Evening 的视觉分组 drop 若会改变时间语义必须产生明确 Time command，禁止只改数组位置。

## Ordering

manual ordering 是上下文相关数据 `ContextOrder`，不是 Todo 单一 global sortIndex。

## Fixtures

reorder、cross-parent move、drop into heading、cancel、edge auto-scroll、selection conflict、expanded row conflict、invalid target。

## Acceptance

drag 过程中 repository write=0；drop 最多一次结构 command；identity 不变；Projection/ContextOrder regression PASS；preview/motion/haptic 可 `VISUAL_PENDING`。
