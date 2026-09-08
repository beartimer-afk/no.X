# H08-A｜OverlayHost + OverlayCoordinator

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

H08-A 只建立 Overlay 基础设施，不实现具体 Picker 内容。

## 平台策略

HarmonyOS 提供 Popup、Dialog、bindSheet、bindContentCover、OverlayManager 等 Root Overlay / Modal 能力；Things V1 为了稳定 Golden、完全控制视觉与坐标，采用 App/Page Root 自有 `Stack + ThingsOverlayHost`，而不是直接套系统默认 Sheet/Dialog 视觉。

## Root Structure

```
MainShell Stack
├─ NavigationContent
├─ GlobalInteractionLayer
└─ ThingsOverlayHost
   ├─ OptionalScrim
   ├─ PrimaryOverlayLayer
   └─ OptionalConfirmationLayer
```

全 App 只有一个 `OverlayCoordinator` 管理 primary overlay。

## Typed Request

`OverlayKind`：WHEN / DEADLINE / REMINDER / TAGS / MOVE / REPEAT / ACTION\_MENU / CONFIRMATION。

`OverlayRequest` 必须包含 requestId、kind、source、typed subject、anchor、placement、modality、dismissPolicy、keyboardPolicy、focusRestorePolicy、typed payload。

Batch Selection 进入 Picker 时 snapshot selected IDs；Overlay 打开不能清空 Selection State。

## Anchor / Placement

Source rect 统一转换为 window/host 坐标，再由 `OverlayPlacementEngine` 根据 SafeRect 计算 ABOVE / BELOW / AUTO / CENTERED 等 final rect。禁止 Picker 内硬编码 x/y 或设备 px。

SafeRect 同时考虑 Window bounds、系统安全区、keyboard avoid area 与 edge inset Token。

## State Machine

```
CLOSED → PREPARING → OPENING → OPEN
OPEN → COMMIT_REQUESTED → COMMITTING → CLOSING → CLOSED
OPEN → CANCELING → CLOSING → CLOSED
OPEN(A) → REPLACING → OPEN(B)
```

最多 1 个 primary + 1 个 confirmation child layer，禁止无限 Overlay stack。

## Domain Boundary

Overlay 内容返回 typed `OverlayResult`；Coordinator / Presenter 再调用 Domain Command。Outside tap / Back / dismiss 永远不能隐式 commit。

## Keyboard / Focus

支持 KEEP / DISMISS\_BEFORE\_OPEN / DISMISS\_AND\_RESTORE\_SOURCE / CUSTOM。ExpandedTodo 属性 Picker 首版推荐先保存 draft 和 selection，再 resign focus、打开 overlay。关闭后按明确 FocusRestorePolicy 恢复 source/editor。

## Interaction Coexistence

* Batch Overlay 不清空 selection；
* Drag 与 Overlay 不同时 active，经过 InteractionArbiter；
* Overlay open 时 MagicPlus 底层不得收到 hit；
* ExpandedTodo 保持同一 todoId、draft 与 scroll context；
* Back 先关闭 confirmation，再 primary，最后才交给 Navigation。

## Hit Test / Z-order

`Page < Global Controls < Scrim < Primary Panel < Confirmation`。Modal overlay 即使 scrim 透明也必须拦截底层 hit。

## Stable IDs

`overlay_host.root/scrim/primary/confirmation`、`overlay_panel.<requestId>`、debug anchor/safe rect IDs。

## Fixtures

closed、anchored below、near-bottom auto-above、left/right clamp、visible/transparent scrim、outside dismiss、explicit-only、keyboard inset、ExpandedTodo source、batch source、confirmation、back、source removed、layout re-anchor、accessibility focus restore。

## Geometry Gate

Panel 必须位于 SafeRect；anchor transform ≤1vp；AUTO placement 能反转；keyboard/layout change 重算；confirmation 始终高于 primary；modal 底层 event count=0。

## 3B Capsules

H08A-01 typed models；02 state machine；03 host skeleton；04 registry；05 anchor transform；06 SafeRect/placement；07 scrim/hit；08 back/outside；09 keyboard/focus；10 selection；11 drag arbiter；12 confirmation；13 re-anchor；14 accessibility；15 VisualLab/tests。

每个任务默认 ≤3 files / ≤200 patch lines。

## Pending

scrim alpha/color、open/close motion、per-picker keyboard/outside policy、anchor gap、edge inset、focus restoration exact iPhone behavior、QuickFind 是否共享 Host。

## 完成定义

single primary、confirmation、placement、安全区、hit interception、Back、keyboard/focus、selection/drag coexistence、ExpandedTodo continuity、Stable IDs、VisualLab/Geometry/Interaction tests 全 PASS 后 H08-A = FUNCTIONAL\_VERIFIED。
