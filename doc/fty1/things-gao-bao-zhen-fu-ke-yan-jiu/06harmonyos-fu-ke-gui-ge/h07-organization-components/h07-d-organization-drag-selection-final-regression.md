# H07-D｜Organization Drag / Selection / Final Regression

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

H07-D 不再增加新的业务实体组件，而是把 ProjectRow、AreaRow、HeadingBlock 放进真实结构后完成 Drag、Move、Selection、Cancel 和 Projection 的组合闭环。

## typed Drag Payload

```
AREA { areaId }
PROJECT { projectId, sourceAreaId? }
HEADING_BLOCK { headingId, sourceProjectId }
```

禁止 Any/Map/JSON payload。

## 状态机

```
IDLE → LONG_PRESS_DETECTING → DRAG_ARMED → DRAGGING
→ TARGET_RESOLVED → DROPPING → COMMITTING → SETTLING → IDLE
```

取消：`DRAGGING → CANCELING → RESTORE_PRESENTATION → IDLE`。Drag state 只属于 Presentation。

## 核心原则

拖动过程中只更新 Presentation order，禁止每帧写 DB、直接修改 areaId/projectId/headingId 或通过 delete+create 模拟 Move。Drop 时只提交一次语义明确的 Domain Command。

所有 target 必须基于实时 rect / midpoint / hierarchy zone 计算，禁止按固定 rowHeight 推算 index。

## Area

拖 Area 时是整个 Area group 的视觉移动；child Projects 不改变 areaId。Drop 后只更新 Area context order。

## Project

同 Area reorder 只更新该 context order；跨 Area Move 必须原子更新 parent + target rank；移出 Area 后 projectId 和 children identities 不变，同时 effective inherited Area tags 重新计算，direct tags 不变。

## HeadingBlock

同 Project reorder 移动完整 block；跨 Project Move 原子迁移 Heading + child Todos 的 Project 语义，headingId / todoIds 全部保持。

## Invalid Drop

Heading→Area/Main Lists、Area→Project/Heading、Project→Heading 内部、Organization→Notes/Checklist 等均 INVALID，command invocation count 必须为 0。

## Gesture Arbitration

Active Drag 独占 pointer；collapse 点击只 collapse；ProjectRow tap 只导航；未 armed 前外层 vertical scroll 正常，armed 后仅由 AutoScroll hook 请求外层滚动。Drag 与 Swipe 不同时 active。

HarmonyOS 可使用 `LongPressGesture + PanGesture + GestureGroup` 组合；平台 500ms 等参数只作为 Seed，不能冒充 Things Token。

## Selection

Selection 是 UI State。selected batch 与 active drag 不得对同一结构执行两次 command。Heading 与 children 由 SelectionNormalizer 去重。Heading direct selectable 的精确 UI 仍 PENDING。

## Collapse

collapsed Area 的 children 不参与当前可见 target geometry；拖入 collapsed Area 是否自动展开仍 PENDING，首版禁止自创 hover-expand。

## Auto Scroll

Coordinator 只发 `onRequestAutoScroll(direction,intensity)`；实际 Page/List ScrollController 属于外层页面。

## ExpandedTodo / Projection Regression

parent structure 变化不得重建 Todo identity 或丢失 draft。每次 Move 后至少重跑 Main Lists、Area、Project、Anytime、Today（若相关）、Quick Find index、effectiveTags、Project progress。Start / Deadline / Completion / Repeat 不得被组织 Move 意外修改。

## Geometry Gate

Project placeholder == source row height ±1vp；Area placeholder == visible Area group height ±1vp；Heading placeholder == HeadingBlock height ±1vp；cancel 后全部 rect 恢复。

## Fixtures

areas\_default、project\_in\_area\_reorder、project\_cross\_area、project\_out\_of\_area、heading\_same\_project、heading\_cross\_project、mixed\_height、cancel、collapsed\_area、selection\_conflict。

## 3B Task Capsule

```
H07D-01 typed payload + state machine
H07D-02 geometry registry / target resolver
H07D-03 Area reorder
H07D-04 Project same-Area reorder
H07D-05 Project cross/out Area
H07D-06 Heading block geometry
H07D-07 Heading same-Project reorder
H07D-08 Heading cross-Project move
H07D-09 invalid drop + cancel
H07D-10 auto-scroll hook
H07D-11 gesture arbitration
H07D-12 selection normalization
H07D-13 projection regression
H07D-14 VisualLab + Geometry tests
```

每个 Capsule 默认 Allowed Files ≤3、Patch Budget ≤200 lines。

## Pending

exact long-press threshold、haptic、preview scale/shadow、target gap visual、collapsed Area auto-expand、auto-scroll threshold/speed、Heading direct selectable、ExpandedTodo parent-drag exact motion。

## H07 完成定义

Core = FUNCTIONAL\_VERIFIED：组件 + reorder/move/cancel/identity/projection regression PASS。

Visual = PARTIAL\_VERIFIED：静态结构可冻结；drag preview/haptic/precise motion/部分 selection visuals 等待 A 级素材。
