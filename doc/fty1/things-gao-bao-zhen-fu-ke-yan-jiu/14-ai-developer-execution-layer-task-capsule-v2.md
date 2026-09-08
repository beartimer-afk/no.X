# 14｜AI Developer Execution Layer｜Task Capsule v2

## 核心原则

总文档负责规划；3B 每次只接收一个 Task Capsule。**3B 不负责决定要读什么。**

## Capsule 必填字段

```yaml
task_id: H02-001
title: TodoCheckbox static states
status: SPEC_READY
owner: 3B_CODER
dependencies:
  - FOUND-TOKEN-LOCK
required_docs:
  - 13 Developer Handbook v2
  - H01 Foundation Tokens
  - H02 TodoCheckbox
optional_docs:
  - 09.2 A-grade evidence index
forbidden_docs:
  - Future page specs
allowed_files:
  - components/todo/TodoCheckbox.ets
  - visual-test/gallery/TodoCheckboxGallery.ets
  - test/TodoCheckbox.test.ets
forbidden_files:
  - domain/**
  - golden/**
  - theme/**
patch_budget:
  files: 3
  lines: 200
fixtures:
  - checkbox_unchecked
  - checkbox_checked
  - checkbox_disabled
acceptance:
  - build PASS
  - three fixtures render
  - stable IDs exist
  - Geometry Contract PASS
  - Golden Diff PASS or PENDING_GOLDEN when baseline not frozen
pending_policy: DO_NOT_GUESS
report_schema: MACHINE_REPORT_V2
```

## 文档优先级

1. Current Task Capsule
2. Canonical Domain Schema / locked technical contract
3. Current H-spec
4. Foundation Token / Acceptance rules
5. Research evidence
6. Historical discussion

发生冲突时，低优先级内容不得覆盖高优先级冻结合同。

## BLOCKED 规则

以下情况必须停止当前 Capsule 而不是猜：缺 Required Doc；依赖未完成；规格标 PENDING 且实现需要该值；需要修改 Forbidden File；Patch Budget 超限；Golden 不存在且 Task 要求真正视觉 VERIFIED；需要用户真机证据。

## Fix Capsule

视觉失败不重新下发整个组件任务，只生成修复 Capsule，例如：

```
TASK H03-FIX-017
ONLY ISSUE: title baseline +2vp
ALLOWED: TodoRow layout token mapping only
DO NOT TOUCH: Checkbox, colors, typography, Domain
PASS: baseline error <=1vp, previous 6 fixtures regression PASS
```

## 目标

让弱模型解决“有明确答案的小约束问题”，而不是要求它理解整个 Things 并进行审美判断。
