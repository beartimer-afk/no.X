# ENG-008｜Inspector Geometry Adapter

**Status:** `SPEC_READY`\
**Dependencies:** ENG-005、ENG-006

## Goal

用 ArkUI 测试/Inspector 能力读取 stable ID 对应的渲染几何，转换成统一 Geometry JSON。

## Required Docs

* H00｜Visual Acceptance Harness
* 07.3｜Golden Screenshot、Geometry 与 Diff
* ENG-006

## Output

```json
{
  "id": "todo_row_todo_normal.title",
  "x": 0,
  "y": 0,
  "width": 0,
  "height": 0,
  "visible": true
}
```

如平台可稳定取得 baseline，增加 `baseline`；不能稳定取得时不得伪造。

## Forbidden

* 找不到组件时不能返回 0,0,0,0
* Adapter 不内置 Todo/Deadline 业务
* Adapter 不决定 tolerance

## Acceptance

* known ID → deterministic rect
* unknown ID → NOT\_FOUND
* 解析异常 → PARSE\_ERROR
* mixed fixtures 可批量输出 JSON
