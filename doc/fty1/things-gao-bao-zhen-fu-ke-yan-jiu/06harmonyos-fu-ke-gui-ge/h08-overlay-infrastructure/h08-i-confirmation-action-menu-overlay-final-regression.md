# H08-I｜Confirmation / Action Menu / Overlay Final Regression

H08-I 不是新的业务弹窗，而是 Overlay 系统最终一致性 Gate。

## Overlay stack contract

全 App 只有一个 OverlayCoordinator。一个业务操作触发第二层确认时必须进入受控 stack；Back 按层级从顶层退出。HarmonyOS 当前官方文档确认 Dialog/Popup/Menu/Sheet/OverlayManager 等均属于 Root 级弹出体系，完全自定义内容/行为时可使用 OverlayManager 类能力；本项目仍保持自有 Things presentation，以避免系统视觉漂移。

## Confirmation

destructive commands（Delete、Heading delete with children、Area structural delete、batch delete 等）由统一 Confirmation model 承载：title/message/actions/destructiveAction/cancelAction。Confirmation 不自行执行 Repository。

## Action Menu

Menu 只展示 Presenter 提供的 actions；enabled/disabled/destructive 来自 ActionPolicy。禁止每个页面复制一份菜单业务判断。

## Keyboard / focus / outside / navigation

每个 Overlay Case 必须声明：open 时 keyboard policy、outside tap policy、Back policy、close 后 focus restore target、页面 route 变化时是否自动 dismiss。未知的 Things 行为标 Pending，不猜。

## Final regression matrix

When、Deadline、Reminder、Tags、Move、Repeat、ActionMenu、Confirmation；来源至少覆盖 ExpandedTodo、multi-select、Project/Area context。检查：z-order、safe area、keyboard avoidance、focus、Back、outside、selection coexistence、drag mutual exclusion、orientation/resize（V1 phone baseline）。

## Motion / visual

Panel surface、row pressed/selected、scrim、radius、shadow、open/close motion 统一走 H01 tokens。不得每个 Picker 写自己的 duration/radius。

## Acceptance

所有 overlay fixtures 可稳定复现；一次只存在合法 stack；关闭不丢 Draft/Selection；不发生 double command；Geometry + Golden + interaction scripts 回归通过后，H08 才能从整体 `IMPLEMENTED` 升为 `VERIFIED`。
