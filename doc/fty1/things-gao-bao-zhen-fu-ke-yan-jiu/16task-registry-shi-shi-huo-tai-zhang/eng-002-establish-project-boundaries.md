# ENG-002｜Establish Project Boundaries

**Status:** `SPEC_READY`\
**Dependency:** `ENG-001 VERIFIED`

## Goal

建立后续所有 3B 任务使用的稳定目录和依赖边界；不写实际业务。

## Required Docs

* 13｜Developer Handbook v2
* 02.8｜Canonical Domain Schema
* ENG-001 Machine Report

## Target Structure

```
entry/src/main/ets/
  app/
  core/
  domain/
    model/
    command/
    query/
  data/
    repository/
    storage/
  presentation/
    presenter/
    pages/
  components/
    foundation/
    todo/
    organization/
    overlay/
  visualtest/
    fixtures/
    gallery/
    geometry/
```

工具放：

```
tools/
  task/
  visual/
  geometry/
  diff/
  report/
```

## Dependency Rules

* `components` 不 import `data/storage/repository`
* `pages` 不直接访问 storage
* `presentation` 通过 Query/Command 与 Domain 交互
* `domain` 不 import ArkUI
* `visualtest` 可以 import Component/Presentation，但不得反向被 production Domain 依赖

## Allowed Files

仅创建目录、空 barrel/interface placeholder、必要 lint/build 配置。

## Acceptance

* Build PASS
* Domain 无 ArkUI import
* Component 无 storage/repository import
* 项目没有循环依赖
* 输出目录树到 Machine Report

若需要改变此目录合同，返回 `BLOCKED_ARCHITECTURE_CHANGE`。
