# H08-C｜WhenPicker

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

WhenPicker 只编辑 Start，不编辑 Deadline。当前官方语义冻结：Today、This Evening、Someday、Calendar OnDate、Clear→Anytime；future start 进入 Upcoming，到日后进入 Today；Reminder 通过 When 设置且只在 Start=OnDate 时合法；iPhone 当前支持在 When date picker 下拉打开 Natural Language Search。

## Domain

```
StartMode = ON_DATE | ANYTIME | SOMEDAY
StartValue { mode, date?: LocalDate, evening: boolean }
```

`evening=true` 仅允许 OnDate(today)。Reminder 是独立字段，日期部分必须等于 StartDate。Deadline 与 When 正交。

## Current Presentation

ANYTIME / SOMEDAY / TODAY / THIS\_EVENING / ON\_DATE(date) / MIXED。Batch subjects 不一致时必须 MIXED，禁止取第一项冒充 current。

## Composition

使用 H08-A Host + H08-B primitives：ShortcutSection（Today/This Evening）、CalendarSection、Someday、Clear/Anytime、ReminderEntrySlot、NaturalLanguageSearchLayer。具体 iPhone 排列和视觉继续 Golden 校准。

## Actions

Today→OnDate(today,false)；This Evening→OnDate(today,true)；Calendar→OnDate(date,false)；Someday→SOMEDAY；Clear→ANYTIME。所有 action 只返回 typed Start result，由 Domain Command 执行。

## Reminder / Repeat / Batch

ReminderEntry 只发 OpenReminderPicker intent。Start→Anytime/Someday 后 Domain 必须清除非法 Reminder。Repeating Copy 的 scope 冲突交给 Repeat Coordinator，不由 WhenPicker 改 Template。Batch 使用单一 batch command，不能 UI forEach 写 DB。

## Natural Language

Search UI 与 parser interface 分离：`parseWhenInput(text,locale,today,timezone)→candidates[]`。3B 不实现自创 parser。支持 Start-only / Start+ReminderTime candidate。

## Calendar

使用自定义 Things calendar presentation，不套系统 DatePicker 最终视觉；日期以 LocalDate/YearMonth 计算，需覆盖 DST。pastDatePolicy 当前 PENDING。

## Stable IDs / Fixtures

包含 Today、Evening、Calendar day、Someday、Clear、Reminder、Search input/suggestions。Fixtures 覆盖各 current 值、batch mixed、reminder、month boundary、DST、search、repeat copy、project subject、keyboard、past policy mock。

## 3B Capsules

Start mapping；shortcut rows；Evening invariant；Calendar model/grid/navigation；date result；mixed selection；Reminder intent；single/batch commands；repeat conflict；Search reveal/input/parser interface/suggestions；keyboard；accessibility；Geometry/Golden；integration regression。

## Pending

exact panel ordering/spacing、selected/calendar visual、month motion、past date policy、reminder time preservation policy、outside behavior、Search pull threshold/motion、parser implementation、exact close timing。

## 完成定义

Core FUNCTIONAL\_VERIFIED：Start modes、single/batch、Domain invariants、Reminder entry、repeat delegation、Calendar/timezone、Search interface、Overlay continuity、accessibility/tests PASS。Visual = PARTIAL\_VERIFIED，精确视觉等 A 级 crop。
