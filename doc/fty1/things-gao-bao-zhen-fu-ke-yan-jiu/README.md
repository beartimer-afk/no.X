# Things 高保真复刻研究

从 iPhone Things Light Mode 产品逆向，到 HarmonyOS 高保真实现与可验证验收体系。

## 这本书解决什么问题

目标不是复制几张截图，而是恢复 Things 背后的产品语义、信息架构、交互模型、数据模型和视觉节奏，并把这些研究转换成能够直接指导 HarmonyOS 开发、能够通过模拟器自动验收的实现规格。

## 内容来源与整理规则

Outlook 项目邮件仍是正式研究事实源；有价值的聊天讨论、真机录屏观察和后续修正也进入书籍。为提高阅读性，不再把邮件整封塞进 RAW 代码块，而是按主题重排为章节、列表、表格和代码块。

整理时必须保留邮件中的：结论、数字、代码、证据等级、Pending、实现限制、验收规则以及结论修正原因。重复的收件箱/已发送副本不重复生成两份正文。

## 当前实现目标

第一复刻目标平台为 HarmonyOS。工程顺序遵循：

**Evidence → Token → Spec → Fixture → Implement → Simulator Capture → Inspector Geometry → Diff → Fix → VERIFIED → Page Composition → Regression**。

这意味着“代码写完”只代表 IMPLEMENTED，不代表完成。
