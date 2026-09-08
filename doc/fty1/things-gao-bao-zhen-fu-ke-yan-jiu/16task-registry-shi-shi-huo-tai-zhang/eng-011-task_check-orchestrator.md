# ENG-011｜task\_check Orchestrator

**Status:** `SPEC_READY`\
**Dependencies:** ENG-002、ENG-006\~010

## Goal

建立单任务统一验收入口，调度器/3B 不再手工决定跑哪些检查。

## Required Docs

* 13｜Developer Handbook v2
* 14｜Task Capsule v2
* 16｜Task Registry

## Conceptual Command

```
./tools/task_check <TASK_ID>
```

实际脚本语言由工程决定，但输入/输出合同冻结。

## Pipeline

1. 读取 Task Capsule。
2. Allowed Files scope check。
3. Build。
4. Task-specific unit tests。
5. Fixture route smoke。
6. Required Geometry checks。
7. Required Screenshot Diff。
8. Required interaction tests。
9. 生成 MACHINE\_REPORT\_V2。

## Forbidden

* task\_check 不修改业务代码
* 不更新 Golden
* 不因为某 Gate 不可用就跳过并判 PASS
* 不自动扩大 Allowed Files

## Acceptance

* 一个 synthetic PASS task 能全链路 PASS
* scope 越界能 FAIL\_SCOPE
* build fail 能准确停止后续无意义 Gate
* visual-required 但 Golden 缺失返回 BLOCKED\_GOLDEN\_BASELINE
* 输出可被 Orchestrator 解析

ENG-011 完成后，Engineering Bootstrap 才可以标记 Milestone 0 `IMPLEMENTATION_READY`。
