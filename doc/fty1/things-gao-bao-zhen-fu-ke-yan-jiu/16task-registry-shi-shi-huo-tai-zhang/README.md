# 16｜Task Registry｜实施活台账

> 实施阶段唯一状态入口。`SPEC_READY` 只表示可以编码。

| Task         | 内容                                                               | 规格          | 实现           | 验收       | 阻塞                      |
| ------------ | ---------------------------------------------------------------- | ----------- | ------------ | -------- | ----------------------- |
| ENG-001\~011 | 工程/Domain/Fixture/VisualLab/截图/Geometry/Diff/Report/Orchestrator | READY       | NOT\_STARTED | NOT\_RUN | ENG-001 需真实开发环境         |
| H01          | Foundation Tokens                                                | SPEC\_READY | NOT\_STARTED | NOT\_RUN | ENG-005                 |
| H02          | TodoCheckbox                                                     | SPEC\_READY | NOT\_STARTED | NOT\_RUN | H01 + VisualLab         |
| H03/H04      | TodoRow / Metadata                                               | SPEC\_READY | NOT\_STARTED | NOT\_RUN | H02                     |
| H05          | ExpandedTodo                                                     | SPEC\_READY | NOT\_STARTED | NOT\_RUN | H03/H04                 |
| H06          | Checklist                                                        | SPEC\_READY | NOT\_STARTED | NOT\_RUN | H05                     |
| H07          | Organization                                                     | SPEC\_READY | NOT\_STARTED | NOT\_RUN | Domain commands         |
| H08-A\~I     | Overlay + When/Deadline/Reminder/Tag/Move/Repeat/Regression      | SPEC\_READY | NOT\_STARTED | NOT\_RUN | H01/H05 + OverlayHost   |
| H09-A        | Selection/Batch                                                  | SPEC\_READY | NOT\_STARTED | NOT\_RUN | TodoRow + Overlay       |
| H09-B        | Magic Plus                                                       | SPEC\_READY | NOT\_STARTED | NOT\_RUN | Commands + page context |
| H09-C        | Todo Drag                                                        | SPEC\_READY | NOT\_STARTED | NOT\_RUN | H07/order commands      |
| H09-D        | Quick Find                                                       | SPEC\_READY | NOT\_STARTED | NOT\_RUN | Query/index layer       |
| H10-A\~F     | Page Composition                                                 | SPEC\_READY | NOT\_STARTED | NOT\_RUN | 组件 + Query              |
| FINAL        | Golden/Geometry/Accessibility/Motion/A-grade calibration         | SPEC\_READY | NOT\_STARTED | NOT\_RUN | 所有实现 + Pending evidence |

## 更新纪律

只有真实文件存在且 Build PASS 才标 `IMPLEMENTED`；只有自动测试/模拟器或真机证据跑过才填验收；`VERIFIED` 必须关联 Report/Golden ID；`LOCKED` 后普通 Capsule 不得修改；`PENDING_EVIDENCE` 必须明确指出缺失证据。
