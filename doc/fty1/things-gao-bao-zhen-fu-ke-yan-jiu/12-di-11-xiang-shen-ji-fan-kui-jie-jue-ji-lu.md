# 12｜第11项审计反馈解决记录

本页逐项回应 3B/外部审计在第 11 章提出的问题，并把反馈转换成工程动作。

| Issue                             | 判断            | 处理                                                                               | 状态                       |
| --------------------------------- | ------------- | -------------------------------------------------------------------------------- | ------------------------ |
| 原始真机证据不在工程资产中                     | Accepted      | 建立 Evidence Registry 与统一目录；无原始素材的 Token 保持 PENDING，不允许 3B 猜                      | OPEN：需用户素材/后续导入          |
| 没有真实 HarmonyOS 工程                 | Accepted / P0 | Engineering Bootstrap 置为开发第一关键路径；先工程、VisualLab、Fixture、Inspector、Diff，再业务组件      | READY                    |
| 页面层与 Todo 交互 H-spec 未完成           | Accepted      | H08-E\~I、Multi-select、MagicPlus、Drag、QuickFind、Page Composition 列入 Task Registry | SPEC\_PENDING            |
| 09.4 状态过期                         | Accepted      | 已改成活台账，区分 SPEC\_COMPLETE / IMPLEMENTED / VERIFIED                                | FIXED                    |
| 缺 Canonical Domain Schema         | Accepted      | 新增 02.8 单一事实源                                                                    | FIXED                    |
| 缺测量/审计工具                          | Accepted / P0 | 新增 Evidence & Measurement Pipeline；Geometry + Screenshot Diff 输出机器报告             | READY FOR IMPLEMENTATION |
| Projection membership 规则未全部冻结     | Partial       | 可确定规则写入 Query Contract；依赖 A 级证据的 Group-by/Evening 等维持 PENDING                    | PARTIAL                  |
| Search / sync / notification 细节不足 | Partial       | V1 优先 Quick Find Index；跨端同步与系统深度通知不作为一期主阻塞                                       | DEFERRED / V1-SCOPE      |

## 审计后的优先级

P0：工程骨架、Canonical Schema、VisualLab/Fixture、Geometry/Diff、Task Capsule 执行层。

P1：Todo Core、Overlay Core、When/Deadline/Reminder/Tag/Move/Repeat。

P2：Organization、Main Views、Quick Find、Selection/Drag/Magic Plus。

P3：精确 Motion/Haptic/视觉 Token A 级校准。

## 结论

审计反馈有效。项目不再用“规格写完≈开发完成”的表述。所有状态必须可追溯到代码、Fixture、截图、Geometry Report 与验收结果。
