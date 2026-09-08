# H07-C｜HeadingBlock

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

Heading 是 Project 内部独立的结构实体，不是 `Todo.isHeading`。`HeadingBlock = HeadingHeader + 当前属于该 Heading 的连续 Todo segment`。

## 领域关系

```
Project
├─ loose Todo*
├─ Heading A
│  └─ Todo*
└─ Heading B
   └─ Todo*
```

Heading 必须拥有稳定 headingId，并且只能属于一个 Project。Todo.headingId 只有在 Todo.parentProjectId 与 Heading.projectId 一致时才合法。

## 官方/真机已确认行为

* Heading 只存在于 Project；
* Move Heading 时其下 Todos 一起移动；
* 可以把 Heading 移动到另一个 Project；
* Duplicate Heading 会复制整个结构组；
* Convert to Project 会把 Heading + children 迁移成新 Project 结构；
* Archive Heading 是结构归档，不等于 Delete；
* Delete Heading 会删除其下 items；
* 已有录屏确认 Project 中可创建 Heading，并存在 selection / Move 相关流程。

## Presentation Structure

```
HeadingBlock
├─ HeadingHeader
│  ├─ Title
│  └─ ContextActions
└─ HeadingChildren
   └─ TodoRow[]
```

HeadingHeader 不允许复用 TodoRow；没有 Completion Checkbox、DeadlineMeta、Todo Metadata。

## Move / Reorder

拖 Heading 的最小结构单位是整个 HeadingBlock。HeadingHeader 不能离开 children 单独移动。跨 Project Move 需要一个原子 Domain Command，并保持 headingId / child todoIds。

## Duplicate / Convert / Archive / Delete

这些全部是独立 Command，不允许通过修改 `type` 或 delete+recreate 模拟。

* Duplicate：新 Heading ID + 新 child Todo IDs；
* Convert：结构迁移并保留可合理保留的数据；
* Archive：保留历史结构，进入 Project logged/archived presentation；
* Delete：破坏性删除 Heading + children，需要相应确认策略。

## Selection

Heading 与 child Todos 的选择语义必须经过 SelectionNormalizer，避免同一 child 因“Heading group + direct item selection”被批量命令处理两次。HeadingHeader 是否可直接进入所有多选流程仍保留 PENDING，3B 不得自行决定。

## Visual / Geometry

Heading title、上下间距、child indent、drag handle、archive appearance 当前部分缺 A 级近景，均 Token 化并保持 `CALIBRATION_REQUIRED`。不允许添加无证据的 Collapse 功能。

## Stable IDs

`heading_<id>.block/header/title/children/context/drag_preview`。

## Fixtures

empty heading、1/3 children、long title、two headings、loose todos + headings、archived、move source/target、convert/duplicate pending、selection conflict。

## 验收

Heading identity、Project scope、group move、cross-project move、Duplicate/Convert/Archive/Delete command boundary、child identity、selection normalization、Project regression 全 PASS 后 Core 可 FUNCTIONAL\_VERIFIED。

## 禁止事项

不得 `Todo.isHeading`；不得 Heading collapse；不得只拖标题；不得跨 Project move 时重建 child Todos；不得 Delete 当 Archive；不得 Convert 当改 enum；不得页面自己修改 heading/project/todo parent 字段。
