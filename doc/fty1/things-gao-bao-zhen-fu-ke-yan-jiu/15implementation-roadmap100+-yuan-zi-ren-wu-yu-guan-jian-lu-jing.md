# 15｜Implementation Roadmap｜100+ 原子任务与关键路径

用户视角可以看 5 个 Milestone，但工程执行仍保持 100+ 原子 Capsule。不要为了减少文档数量把任务重新变成大而模糊的提示词。

## Milestone 0｜Engineering Bootstrap

ENG-001 工程创建；ENG-002 目录/模块边界；ENG-003 Canonical Domain interfaces；ENG-004 Fixture Registry；ENG-005 DevVisualLab route；ENG-006 Stable ID convention；ENG-007 screenshot capture；ENG-008 Inspector geometry capture；ENG-009 diff CLI；ENG-010 machine report；ENG-011 CI/本地回归入口。

## Milestone 1｜Foundation + Todo Core

FOUND-001~~005（Text/Icon/Divider/Row/Button）；H02-001~~005 Checkbox；H03/H04 metadata + TodoRow；H05-B/C1/C2/D；H06-A/B/C。按“最小状态→Gallery→Golden→锁定→组合”的依赖执行。

## Milestone 2｜Attribute / Overlay

H08-A/B 已具备规格；H08-C When、H08-D Deadline；继续 H08-E Reminder、F Tag、G Move、H Repeat、I Confirmation/ActionMenu/Regression。Picker 只能组装 Overlay primitives，不得自己重建 Panel 系统。

## Milestone 3｜Organization + Global Interaction

H07 A/B/C/D；SelectionController、BatchActionBar、MagicPlus、Todo DragCoordinator、QuickFind 的正式 H-spec + Task Capsules。

## Milestone 4｜Page Composition

Main Lists、Inbox、Today、Upcoming、Anytime、Someday、Logbook、Area、Project、Quick Find。每页必须先建立固定 Page Fixture，再 Page Golden，再交互脚本。

## Milestone 5｜Final Regression / A-grade Calibration

全量 Golden、Geometry、Accessibility、Motion scripts、真机 A 级校准、Pending 清零/豁免、Release Candidate。

## 当前关键路径

`ENG-001 → ENG-004/005/006 → ENG-007/008/009 → H01 implementation → H02 → TodoRow → ExpandedTodo → Overlay → Pickers → Pages → final regression`

## 并行路径

Domain/Query/Command interfaces 可以与 Visual Harness 并行；A 级证据采集可独立推进，不得阻塞不依赖精确 Token 的 FUNCTIONAL implementation。

## 规则

每个 Capsule 完成后由调度器更新 Registry；3B 不自行挑选下一任务。
