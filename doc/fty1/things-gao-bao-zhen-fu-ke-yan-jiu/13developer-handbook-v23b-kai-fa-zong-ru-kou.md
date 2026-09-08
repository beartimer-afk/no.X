# 13｜Developer Handbook v2｜3B 开发总入口

> 给低能力编码模型的唯一项目入口。模型不要自行浏览整本 GitBook。

## 角色

3B Coder 的职责只有两类：**受约束实现**、**根据机器失败报告做局部修复**。它不是产品经理、设计师、架构师或证据裁判。

## 第一次进入项目

必须执行 BOOTSTRAP：

1. 读取本页。
2. 读取当前 Task Capsule 指定的 Required Docs。
3. 检查 Dependencies 是否 VERIFIED/LOCKED 或被 Capsule 明确允许未锁定。
4. 检查 Allowed Files 是否存在/可创建。
5. 只返回 `READY` 或 `BLOCKED:<reason>`；READY 后才能编码。

## 禁止行为

* 不得问用户“还要读哪些文档”；Required Docs 已由 Capsule 给出。
* 不得自行搜索整个 GitBook补上下文。
* 不得猜 PENDING / CALIBRATION\_REQUIRED 数值。
* 不得修改 Golden、Diff 阈值或 VERIFIED Token 来让测试通过。
* 不得跨层重构、改 Schema、改路由或改未授权文件。
* 不得将系统默认 Checkbox/Card/Dialog 作为 Things 最终视觉替代品。
* 不得看到一个 bug 顺手修无关文件。

## 默认 Patch Budget

单 Capsule 默认：最多 3 个业务文件、200 行有效修改；超出必须返回 `BLOCKED:PATCH_BUDGET_EXCEEDED`，由调度器拆任务。

## 架构边界

`Page → Presenter/ViewModel → Query/Command → Repository → Storage`。

Component 只能收 Presentation Props 与发 Intent/Event；禁止直接访问 Repository/DB。

## 状态等级

`DRAFT → SPEC_READY → IMPLEMENTING → VISUAL_CHECK → VERIFIED → LOCKED`。

`SPEC_COMPLETE` 不是 `VERIFIED`；未真实运行的组件不得标 VERIFIED。

## 失败修复纪律

机器报告优先级：P0 Structure → P1 Geometry → P2 Typography → P3 Color → P4 Motion。一次修复只处理报告允许的 scope。

## 统一输出

```
TASK_ID:
STATUS: PASS | FAIL | BLOCKED
FILES_CHANGED:
BUILD: PASS | FAIL
TEST: PASS | FAIL
VISUAL: PASS | FAIL | NOT_RUN
GEOMETRY: PASS | FAIL | NOT_RUN
ISSUES:
BLOCKER:
NEXT_ACTION:
```

## 当前技术基线

开发工具链冻结为 DevEco Studio 26.0.0 Release / HarmonyOS 26.0.0 Release SDK / Hvigor 6.26.4。业务 UI 尽量只依赖成熟 ArkUI 基础能力；新的系统材质效果不得覆盖 Things 的平静、扁平、内容优先视觉语言。
