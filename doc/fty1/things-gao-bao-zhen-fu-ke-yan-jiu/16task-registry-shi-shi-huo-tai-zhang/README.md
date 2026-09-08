# 16｜Task Registry｜实施活台账

> 这是实施阶段唯一任务状态入口。`SPEC_READY` 表示可以编码，不代表代码存在。

| Task         | 内容                                  | 规格            | 实现           | 验收       | 阻塞                       |
| ------------ | ----------------------------------- | ------------- | ------------ | -------- | ------------------------ |
| ENG-001      | HarmonyOS 26 工程初始化                  | READY         | NOT\_STARTED | NOT\_RUN | 本机开发环境待执行时确认             |
| ENG-002      | 模块/目录边界                             | READY         | NOT\_STARTED | NOT\_RUN | ENG-001                  |
| ENG-003      | Canonical Domain interfaces         | READY         | NOT\_STARTED | NOT\_RUN | ENG-001                  |
| ENG-004      | Fixture Registry                    | READY         | NOT\_STARTED | NOT\_RUN | ENG-001                  |
| ENG-005      | DevVisualLab                        | READY         | NOT\_STARTED | NOT\_RUN | ENG-001                  |
| ENG-006      | Stable ID convention                | READY         | NOT\_STARTED | NOT\_RUN | ENG-005                  |
| ENG-007      | Simulator screenshot capture        | READY         | NOT\_STARTED | NOT\_RUN | ENG-005                  |
| ENG-008      | Inspector geometry capture          | READY         | NOT\_STARTED | NOT\_RUN | ENG-005/006              |
| ENG-009      | Screenshot diff/report              | READY         | NOT\_STARTED | NOT\_RUN | ENG-007                  |
| H01          | Foundation Tokens implementation    | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | ENG-005                  |
| H02          | TodoCheckbox                        | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | H01 + VisualLab          |
| H03/H04      | TodoRow / Metadata                  | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | H02                      |
| H05          | ExpandedTodo                        | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | TodoRow                  |
| H06          | Checklist                           | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | H05 container            |
| H07          | Organization                        | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | Domain commands          |
| H08-A\~D     | Overlay/When/Deadline               | SPEC\_READY   | NOT\_STARTED | NOT\_RUN | Foundation + OverlayHost |
| H08-E\~I     | Reminder/Tag/Move/Repeat/Regression | SPEC\_PENDING | NOT\_STARTED | NOT\_RUN | 强模型补规格                   |
| GLOBAL-SEL   | Multi-select/Batch                  | SPEC\_PENDING | NOT\_STARTED | NOT\_RUN | 强模型补规格                   |
| GLOBAL-MAGIC | Magic Plus                          | SPEC\_PENDING | NOT\_STARTED | NOT\_RUN | 强模型补规格                   |
| GLOBAL-DRAG  | Todo Drag                           | SPEC\_PENDING | NOT\_STARTED | NOT\_RUN | 强模型补规格                   |
| GLOBAL-QF    | Quick Find                          | SPEC\_PENDING | NOT\_STARTED | NOT\_RUN | 强模型补规格                   |
| PAGE-\*      | 主要页面组合                              | SPEC\_PENDING | NOT\_STARTED | NOT\_RUN | 组件/Query完成               |

## 状态更新纪律

* 只有真实文件存在且构建通过才能标 `IMPLEMENTED`。
* 只有自动测试/模拟器证据跑过才填验收结果。
* `VERIFIED` 必须记录报告或 Golden ID。
* `LOCKED` 之后一般任务不得修改。
* `PENDING_EVIDENCE` 需要明确指出哪段真机证据缺失。
