# ENG-003｜Canonical Domain Interfaces

**Status:** `SPEC_READY`\
**Dependencies:** ENG-001、ENG-002

## Goal

将 GitBook 02.8 的 Canonical Domain Schema 转换为纯 ArkTS Domain 类型。这里只建立类型和不变量测试，不实现 UI/DB。

## Required Docs

1. 13｜Developer Handbook v2
2. 02.8｜Canonical Domain Schema｜单一事实源

## Allowed Files

* `domain/model/**`
* 与 Domain 类型直接相关的纯单测

## Required Types

* `LifecycleStatus`
* `StartMode`
* `Todo`
* `Project`
* `Area`
* `Heading`
* `ChecklistItem`
* `Tag`
* `RepeatSeries`
* `ContextOrder`
* `ReminderSpec`

## Forbidden

* 不新增 `isToday / isUpcoming / selected / expanded / dragging`
* 不把 Someday 写成 null
* 不给 Area 加 completed/canceled
* 不让 Project 嵌套 Project
* 不实现 Repository
* 不 import ArkUI

## Acceptance

* Type compile PASS
* Domain package 内无 ArkUI import
* 不变量单测 PASS
* `StartMode.OnDate` 必须携带 LocalDate 语义
* Todo headingId 的文档约束保留，运行时 validation 可后续任务实现

若 02.8 与其他文档冲突，以 02.8 为准并报告冲突。
