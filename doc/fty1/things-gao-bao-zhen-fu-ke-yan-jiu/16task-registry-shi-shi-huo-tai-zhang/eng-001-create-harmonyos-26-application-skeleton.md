# ENG-001｜Create HarmonyOS 26 Application Skeleton

**Status:** `SPEC_READY`\
**Owner:** `3B_CODER`\
**Dependencies:** none

## Goal

创建可以在 HarmonyOS 26 模拟器运行的最小 ArkTS/ArkUI 应用骨架。**只建工程，不实现 Things 业务 UI。**

## Required Docs

1. 13｜Developer Handbook v2
2. 15｜Implementation Roadmap（Milestone 0）
3. 本 Capsule

## Forbidden Docs

H02-H08、页面研究、未来功能规格。当前任务不需要读取它们。

## Allowed Files

* 项目级 `build-profile.json5`
* `hvigor*`
* `oh-package.json5`
* `entry/build-profile.json5`
* `entry/src/main/module.json5`
* `entry/src/main/ets/**` 中最小 bootstrap 页面
* DevEco 自动生成且为工程启动必需的资源文件

## Forbidden

* 不创建 Todo/Project/Area Domain 业务逻辑
* 不创建 TodoRow/Checkbox/Overlay/Picker
* 不引入第三方依赖
* 不修改 Golden
* 不复制 Things 设计图到首页

## Frozen Baseline

* DevEco Studio 26.0.0 Release
* HarmonyOS 26.0.0 Release SDK
* `compileSdkVersion = 26.0.0`
* Hvigor/hvigorw 6.26.4 对应工具链
* ArkTS + ArkUI

## Steps

1. 用当前正式模板创建 Phone 应用。
2. 移除与最终产品无关的模板 demo 文字/图片，只保留中性 bootstrap surface。
3. 确认工程 wrapper/build config 完整。
4. Debug build。
5. 在固定 Phone 模拟器启动。
6. 保存 `ENG-001-home.png`。

## Acceptance

* Build PASS
* Emulator launch PASS
* 无模板示例业务残留
* 应用可重新冷启动
* `ENG-001-home.png` 存在

## Report

按 `MACHINE_REPORT_V2` 输出。若本机没有 DevEco/SDK/模拟器，返回 `BLOCKED_ENVIRONMENT`，不得改 SDK 目标版本。
