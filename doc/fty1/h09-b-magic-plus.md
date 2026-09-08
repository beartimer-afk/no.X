# H09-B｜Magic Plus

Magic Plus 是直接操纵式创建入口，不是普通固定 FAB。

## Intent model

`CreateIntent { entityType, context, insertionTarget, initialParent?, initialHeading?, initialStart? }`。MagicPlus 只产生 CreateIntent；创建后的 Domain identity、默认字段与排序由 Command 决定。

## Context

在 Inbox/Today/Project/Area 等上下文中，tap 与 drag/drop 可形成不同 insertion intent。Heading insertion target 与 Todo insertion target 必须区分；禁止 UI 通过屏幕 y 坐标直接写 parentId。

## Drag targeting

Presentation 维护 hover target / insertion indicator / preview，drop 后一次性提交 Create command。取消 drag 不产生实体。

## Pending

精确吸附阈值、preview scale、haptic、auto-scroll、不同页面的所有 target 视觉需要真机/Golden 校准；3B 只实现已冻结 target contract。

## Acceptance

创建位置正确、parent/time context 不串线、manual order 可预测、cancel no-op、Stable ID/fixture/drag script PASS。
