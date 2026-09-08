# H08-D｜DeadlinePicker

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

DeadlinePicker 只编辑 Deadline，不编辑 Start / Evening / Reminder。

## 已确认事实

A 级真机确认：独立深色 Picker、Deadline Today、与 Today / This Evening 共存、Batch Deadline。官方当前确认：Deadline 表示最晚完成日期；有 Deadline 的 item 可保持 Anytime；Clear 移除 Deadline；Deadline 无 Reminder；Todo / Project 均支持；Deadlines 特殊列表按日期组织。

## Domain

`DeadlineValue = LocalDate | None`。没有 Evening、ReminderTime、duration 或 time-of-day。UI 以 LocalDate 语义处理，禁止 UTC midnight 直接充当用户日期。

## 正交性

SetDeadline 不能改 Start、Evening、Reminder、Parent、Repeat。Deadline=today 后由 TodayQuery 重新投影，不允许 UI 自己 addToToday。

## Presentation

NONE / ON\_DATE(date) / MIXED。Batch deadline 不一致时必须 MIXED。

## Panel

使用 H08-A Host + H08-B `DARK_PICKER` surface。功能由 Today/Calendar/Clear 组成；精确排序、calendar visual、圆角/阴影继续 Golden 校准。禁止 Someday、Anytime row、Reminder、iPhone Natural Language Search。

## Actions

Today→SetDeadline(today)；Calendar date→SetDeadline(date)；Clear→Deadline None。所有 action 只返回 typed result，再由 Domain Command 提交。

## Reminder / Search

Deadline 不允许 Reminder。当前官方没有 iPhone Deadline Natural Language 的同等支持说明，V1 Phone 不从 WhenPicker 复制 Search。

## Overdue / Validation

Overdue 是 Presenter 计算结果，不存 `isOverdue`。过去日期选择策略、deadline < future Start 时的产品策略当前保持 PENDING；Picker 不得自动移动 Start。

## Batch / Repeat / Project

Batch 使用单一 `Set/ClearDeadlineForItemsCommand`，不能 UI forEach 写 Repository。Repeating Copy 的 single/template scope 冲突交给 Repeat Coordinator。Project Deadline 不级联到 children。

## Shared DateGridCore

When / Deadline 只复用日期网格与 date math，不复用整个 Picker ViewModel。Deadline DateGrid 没有 Start/Evening/Someday/Reminder 业务。

## Projection Regression

Set/Clear 后重跑 Today、Deadlines 特殊列表、DeadlineMeta、Project presentation；identity 不变。

## Stable IDs / Fixtures

覆盖 none/today/future/overdue、batch none/same/mixed、Project、repeat copy、Today+Evening+Deadline、Anytime+Deadline、Clear、month/DST、policy mocks、ExpandedTodo source。

## 3B Capsules

Deadline current/mixed mapping；DateGridCore extraction；dark panel fixture；selected state；Today；Clear；single/batch commands；Project；repeat delegation；overdue；DeadlinesQuery / DeadlineMeta regression；validator hooks；accessibility；Geometry/Golden；integration regression。

## Pending

panel geometry、calendar cells、selected/overdue visuals、past selection policy、deadline-before-start policy、Today shortcut exact layout、outside behavior、month motion、repeat scope exact UI。

## 完成定义

Core FUNCTIONAL\_VERIFIED：None/Date、Today、Calendar、Clear、single/batch、Todo/Project、Start/Reminder isolation、repeat delegation、DateGridCore、Deadlines projection、DeadlineMeta、timezone、accessibility/tests PASS。

Visual PARTIAL\_VERIFIED：dark panel 方向已 A 级确认；精确视觉与 motion 等 A 级 crop + simulator Golden。
