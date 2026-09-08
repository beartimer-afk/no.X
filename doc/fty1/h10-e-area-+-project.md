# H10-E｜Area + Project

## Area

Area 是长期责任容器，不可 Complete。Area 页面 Query 返回 direct Todos + Projects；Collapse 是 presentation state。Delete Area 是 destructive structural command，不等同 remove-parent。

## Project

Project 有 Open/Completed/Canceled 生命周期，IsLogged 独立。页面组合 ProjectHeader、loose Todos、HeadingBlock、TodoRow、Project metadata/Deadline presentation。Heading 只属于一个 Project；Heading Move/Archive/Convert/Delete 使用 H07 Commands。

Project close 时存在未完成 children 的精确当前 iPhone 流程仍 `PENDING_EVIDENCE`，必须通过 `ProjectClosePolicy` interface 隔离，3B 不得自创对话。

Fixture：Area direct todos + 2 projects；Project loose todos + Heading A/B；project deadline；completed/canceled policy mocks；heading move target。Golden 检查 title/header、Heading rhythm、child indentation、bottom create controls。
