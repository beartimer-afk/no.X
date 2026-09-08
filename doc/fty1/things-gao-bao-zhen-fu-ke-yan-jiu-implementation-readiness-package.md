# Things 高保真复刻研究 - Implementation Readiness Package

> 更新时间：2026-09-09

## 项目阶段

* Research：COMPLETE
* Specification：COMPLETE
* Implementation Preparation：COMPLETE
* Implementation：NOT\_STARTED
* Verification：NOT\_STARTED

## Engineering Capsules

### ENG-001 Project Bootstrap

定义 HarmonyOS 工程结构、模块边界和依赖方向。

状态：SPEC\_READY

### ENG-004 Fixture System

定义确定性测试数据、Scenario 和稳定 ID。

状态：SPEC\_READY

### ENG-005 Visual Harness

定义页面启动、截图采集、Geometry Collector 和视觉验证框架。

状态：SPEC\_READY

### ENG-006 Screenshot Pipeline

定义 Screenshot Artifact、Pixel Diff、Geometry Diff 和 Evidence Package。

状态：SPEC\_READY

## Page Specifications

已完成规格：

* H01 Today
* H02 Todo Editor
* H03 Upcoming
* H04 Inbox
* H05 Projects
* H06 Areas
* H07 Search
* H08 Settings

状态：SPEC\_READY

## Implementation Roadmap

Phase 0：HarmonyOS 工程初始化

Phase 1：Core + Today 垂直闭环

Phase 2：Editor + Inbox 用户生命周期

Phase 3：Projects / Areas / Search / Settings

## Verification Rules

禁止将未编码内容标记 IMPLEMENTED。 禁止将无证据内容标记 VERIFIED。 PENDING\_EVIDENCE 不猜测。

## Current Blockers

* HarmonyOS DevEco 工程环境
* Things 真机视觉证据

下一关键路径：真实工程创建与 H01 第一轮实现。
