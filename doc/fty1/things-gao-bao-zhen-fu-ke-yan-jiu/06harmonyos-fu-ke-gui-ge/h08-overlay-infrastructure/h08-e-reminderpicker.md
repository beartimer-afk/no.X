# H08-E｜ReminderPicker

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

ReminderPicker 只编辑提醒时间，不编辑 Deadline，也不把 Reminder 建模成第二个日期字段。

## Domain invariant

Reminder 依附 Start Date：只有 `StartMode=ON_DATE` 时 Reminder 才合法。Domain 建议保存 `ReminderValue = LocalTime | None`，提醒的 LocalDate 由 StartDate 决定；禁止把 `deadline - 1h`、`before deadline` 等未经 Things 证据确认的模型塞进 V1。

当 Start 从 OnDate 变为 Anytime/Someday 时，Command 层必须处理 Reminder 失效；Picker 本身不得偷偷保留非法 Reminder。

## Composition

使用 H08-A OverlayHost / Coordinator 与 H08-B Panel primitives。Presentation 至少包含 current time、time selection、clear。快捷时间行、滚轮/列表形态、关闭策略和动画保持 `CALIBRATION_REQUIRED`，3B 不得自行设计最终视觉。

## Command contract

`SetReminderTime(subjectIds, localTime)` / `ClearReminder(subjectIds)`。Single 与 Batch 都通过一个 typed command 提交；UI 禁止逐条直接写 Repository。

## Interaction

从 WhenPicker 的 ReminderEntry 或 ExpandedTodo 属性入口打开。打开前由 Presenter 提供 StartDate/current Reminder；若 Start 不是 OnDate，则返回 `BLOCKED_BY_START` presentation state，由上层决定是否引导先设 When，ReminderPicker 不自动改 Start。

## Fixture / Stable ID

覆盖 none、09:00、18:00、batch same、batch mixed、Start missing、keyboard/time selector、DST boundary。Stable ID 至少包括 panel、time control、clear row、confirm/commit action（若最终呈现需要确认）。

## Acceptance

功能 Gate：Start/Reminder invariant、single/batch command、clear、timezone/DST、focus restore、Back/outside 行为、accessibility。视觉 Gate：Panel geometry、time selector、selected state、motion 必须经 simulator Golden；未校准时只能 `FUNCTIONAL_VERIFIED / VISUAL_PENDING`。
