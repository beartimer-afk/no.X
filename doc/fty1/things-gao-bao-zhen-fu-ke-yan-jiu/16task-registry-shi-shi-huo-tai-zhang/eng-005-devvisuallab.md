# ENG-005｜DevVisualLab

**Status:** `SPEC_READY`\
**Dependencies:** ENG-001、ENG-002、ENG-004

## Goal

建立 dev-only 视觉实验室，用固定 route + fixture 单独渲染组件/页面。

## Required Docs

* H00｜Visual Acceptance Harness
* 07.2｜DevVisualLab 与 Fixture
* 13｜Developer Handbook v2

## Allowed Files

* `visualtest/gallery/**`
* dev-only route/entry 配置

## Initial Routes

```
/dev/foundation?fixture=foundation_empty
/dev/checkbox?fixture=checkbox_unchecked
/dev/todo-row?fixture=todo_normal
/dev/expanded?fixture=expanded_empty
/dev/project?fixture=project_standard
```

路由形式可按 ArkUI 工程实现适配，但 fixture ID 必须稳定。

## Forbidden

* 不允许 DevVisualLab 连接生产数据库
* 不在 VisualLab 内写业务逻辑
* 不根据当前时间随机展示状态

## Acceptance

* 所有初始 route 可打开
* 同一路由重复打开渲染稳定
* 未知 fixture 显示明确开发错误，不 fallback 到随机数据
* production release 可排除 DevVisualLab 入口
