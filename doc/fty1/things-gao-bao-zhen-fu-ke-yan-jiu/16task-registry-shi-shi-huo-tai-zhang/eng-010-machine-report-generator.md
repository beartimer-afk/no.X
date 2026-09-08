# ENG-010｜Machine Report Generator

**Status:** `SPEC_READY`\
**Dependencies:** ENG-007、ENG-008、ENG-009 可逐步接入

## Goal

把 Scope/Build/Test/Geometry/Visual 结果汇总成固定 `MACHINE_REPORT_V2`，让 Orchestrator 可以机器判断下一步。

## Required Docs

* 13｜Developer Handbook v2
* 14｜AI Developer Execution Layer

## Required Fields

```
TASK_ID
STATUS
FILES_CHANGED
BUILD
TEST
VISUAL
GEOMETRY
FIXTURES_RUN
ISSUES
BLOCKER
NEXT_ACTION
```

同时输出 JSON 版本供自动调度。

## Status Rules

* 任一 required Gate FAIL → PASS 不成立
* 环境/依赖缺失 → BLOCKED，不伪装 FAIL\_VISUAL
* 没跑 Visual → `NOT_RUN`，不得写 PASS
* 只有 Orchestrator 可以将 task 状态更新为 VERIFIED/LOCKED

## Acceptance

* 所有状态组合 schema validation PASS
* 缺 required 字段 → report generation FAIL
* 人类文本与 JSON 语义一致
