# 27｜Git 提交与回滚协议：每个 TASK 一个可回退点

## 目的

3B 模型常见问题是修一个页面把之前页面改坏。因此每个任务必须独立提交。

## 开始任务前

必须：

1. 工作区 clean；
2. 当前分支构建 PASS；
3. 上一个 TASK 已验收；
4. 记录当前 commit hash。

## Commit 规则

每个任务通过验收后提交一次：

```
NOX TASK-001 home static
NOX TASK-002 round ready
NOX TASK-003 bomb waiting
...
```

禁止一个 commit 包含两个 TASK。

## 修复提交

如果 TASK 已提交后发现验收 bug：

```
NOX FIX TASK-00X <short issue>
```

## 发生严重偏离时

例如模型重新设计了全局组件：

1. 停止；
2. 保存当前 diff 供审查；
3. 回滚到最近通过 TASK 的 commit；
4. 重新发送对应 TASK；
5. 不在错误分支上继续堆修复。

## 禁止

* 自动 squash 全历史；
* 一个巨大“implement app”提交；
* 未验收就 commit 为完成；
* 为修当前 TASK 改历史 TASK 的规格值。

## 验收

最终 Git log 应清晰对应 TASK-001 至 TASK-017。
