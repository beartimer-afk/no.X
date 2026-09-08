# 01 Engineering Handbook 架构设计

## 核心原则

采用 Domain First，而不是 Page First。

结构：

UI → State → Command → Repository → Model

## 核心模块

* Todo
* Project
* Area
* RepeatRule

## 数据修改规则

禁止页面直接修改业务状态。

所有变化通过 Command 完成：

* CreateTodo
* CompleteTodo
* UpdateTodo
* DeleteTodo
