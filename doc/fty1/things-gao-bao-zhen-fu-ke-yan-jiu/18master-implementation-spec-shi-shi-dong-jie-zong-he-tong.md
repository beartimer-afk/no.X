# 18｜Master Implementation Spec｜实施冻结总合同

本页是 HarmonyOS 实施阶段的最高级工程合同。它不替代细规格，而是规定冲突优先级、里程碑 Gate 和“不能简化成什么”。

## Source priority

1. 当前 Task Capsule
2. Canonical Domain Schema / Master Contract
3. 当前 H-spec
4. H00/H01 Acceptance + Tokens
5. A/B/C Evidence
6. 历史讨论

低优先级不得覆盖高优先级。

## Architecture

`Page → Presenter/ViewModel → Query/Command → Repository → Storage`；Component 只接 Presentation Props、发 Intent/Event。Projection 不是 parent；UI transient state 不进入 Domain。

## 不可破坏模型

Start(When) 与 Deadline 正交；Reminder 依附 StartDate；This Evening 是 Today sub-bucket；Area 可 direct Todo + Project；Heading 只属 Project；ChecklistItem 不是 Todo；Repeat=Template/Series/Copy；Logbook=Projection；manual order=context-specific；Selection=UI state。

## Milestone gates

M0 Engineering Bootstrap → M1 Foundation/Todo Core → M2 Overlay/Attributes → M3 Organization/Global Interaction → M4 Page Composition → M5 Final Regression/A-grade Calibration。

每个 Milestone 内可有 100+ 原子 Capsule。用户不需要逐个确认；调度器自动选择依赖满足的 READY Capsule。

## Status truth

`SPEC_READY` 只表示可以编码；`IMPLEMENTED` 必须有真实文件+build pass；`VERIFIED` 必须有 fixture + simulator/real-device evidence + automated gate pass；`LOCKED` 才是可作为下游稳定依赖的组件。

## Pending policy

任何 `PENDING_EVIDENCE / CALIBRATION_REQUIRED` 不得由 3B 猜。可以先完成功能架构并标 `VISUAL_PENDING`，但禁止伪造 Golden PASS。

## HarmonyOS platform rule

使用成熟 ArkUI 布局/输入/手势/可访问性能力，但不自动采用 HarmonyOS 默认组件视觉或新的沉浸光感材质作为 Things 视觉。系统能力是实现工具，Things 真机证据是视觉目标。

## Release condition

只有全量关键 Fixture、Golden、Geometry、Interaction、Accessibility、Motion/gesture scripts 通过，且 P0/P1 Pending 已清零或正式豁免，才可进入 Release Candidate。
