# 05 Audit Log - 项目审计记录

## 状态规则

项目严格区分：

* SPEC\_READY：规格已定义
* IMPLEMENTED：代码已完成
* VERIFIED：有实际验证证据
* PENDING\_EVIDENCE：等待证据
* BLOCKED：存在外部依赖

## 2026-09-05 项目启动

确定 Things iPhone Light Mode 高保真复刻目标。

范围：

* iPhone
* Light Mode
* 当前版本
* 可跨平台复用能力

排除：

* iPad
* 深度 iOS 系统绑定能力
* 暂不迁移的系统特性

## 2026-09-08 架构收敛

确定：

Domain First

Core 与 UI 解耦

Command 驱动状态变化

Repository 抽象数据来源

## 当前 Blocker

1. HarmonyOS 实际工程环境
2. Things 真机视觉证据
3. 页面 Geometry 数据

## 审计原则

禁止把设计稿标记为实现完成。 禁止把推测标记为验证完成。
