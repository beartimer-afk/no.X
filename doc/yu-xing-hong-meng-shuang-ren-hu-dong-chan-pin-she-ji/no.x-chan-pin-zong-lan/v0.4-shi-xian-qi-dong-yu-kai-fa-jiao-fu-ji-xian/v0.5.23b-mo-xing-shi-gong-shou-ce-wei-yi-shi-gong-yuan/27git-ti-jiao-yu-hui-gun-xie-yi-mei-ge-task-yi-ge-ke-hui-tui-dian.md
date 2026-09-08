# 27｜Git 提交与回滚协议：每个 TASK 一个可回退点

## 目的

弱模型修一个页面时可能破坏之前内容，因此每个任务必须独立提交。

## 开始任务前

1. 工作区 clean；
2. 当前分支构建 PASS；
3. 上一 TASK 已人工验收；
4. 记录当前 commit hash。

## Commit 规则

资源前置任务：

```
NOX TASK-000A resources and font freeze
NOX TASK-000B layered app icon
```

业务任务：

```
NOX TASK-001 home static
NOX TASK-002 round ready
NOX TASK-003 bomb waiting
...
NOX TASK-017 ui qa
```

禁止一个 commit 包含两个 TASK。

## 修复提交

`NOX FIX TASK-00X <short issue>`

## 严重偏离

停止 → 保存 diff → 回滚最近 PASS commit → 重新发送该 TASK → 禁止在错误分支继续堆修复。

## 禁止

自动 squash 全历史、巨大 implement app 提交、未验收先 commit 为完成、为修当前 TASK 改历史规格值。

## 验收

最终 Git log 必须能看到 TASK-000A、000B、001 至 017 的独立提交链。
