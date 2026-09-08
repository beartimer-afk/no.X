# ENG-006｜Stable ID Convention

**Status:** `SPEC_READY`\
**Dependencies:** ENG-005

## Goal

统一所有 Geometry/UI Test 使用的稳定组件 ID，禁止测试依赖文本搜索或组件层级位置猜测。

## Required Docs

* H00｜Visual Acceptance Harness
* 07.1｜组件完成定义与 28 段规格模板
* 07.3｜Golden Screenshot、Geometry 与 Diff

## ID Format

```
<component>_<entity-or-fixture>.<part>
```

例如：

```
todo_checkbox_fixture1.root
todo_checkbox_fixture1.visual
todo_checkbox_fixture1.hit
todo_row_todo_normal.title
todo_row_todo_normal.metadata
```

## Rules

* ID 不能含动态 index 作为唯一 identity
* 同一 fixture 多次运行 ID 不变
* visual node 与 hit-target node 分离时必须有独立 ID
* production 可保留必要 stable ID，不把 debug 数据写入 Domain

## Acceptance

* Known ID 可被 Inspector/Test API 定位
* Unknown ID 返回 NOT\_FOUND
* reorder 后 entity-based ID 不变
* ID helper 有纯单测
