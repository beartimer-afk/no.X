# 19｜TASK-016：端到端核心闭环验收

## 目标

本任务原则上**不新增功能**，只验证和修复此前任务组合后的完整链路。

## 依赖

TASK-001 至 TASK-015 全 PASS。

## 固定测试脚本 A：正常完整一轮

严格执行：

1. 冷启动；
2. Home 截图；
3. 点击开始游戏；
4. Round Ready 截图；
5. 点击我准备好了；
6. Bomb Waiting 截图；
7. 按住 >1100ms；
8. 合法松手；
9. Handoff；
10. 下一人按住；
11. 至少经历 2 次 Handoff；
12. 等待自然 Explosion；
13. 观察 1180ms 动效；
14. Lose Reveal；
15. 等 1100ms；
16. 点击查看挑战；
17. Challenge Reveal；
18. 点击去完成；
19. 点击下一轮；
20. 进入 ROUND 2。

任何一步与流程不同即 FAIL。

## 固定测试脚本 B：短按失败

* Bomb 中按 300–700ms 松手；
* 必须回 Waiting；
* 显示 `再按久一点。`；
* holder 不变；
* bombExplodeAt 不变。

## 固定测试脚本 C：滑出取消

* 按住后滑出命中区超过 12vp；
* 视为取消；
* holder 不变；
* 不 Handoff。

## 固定测试脚本 D：Fake Signal Pressing

* 通过调试模式让 Fake 在 Pressing 触发；
* 手指保持；
* 按压时长继续；
* Fake 结束后仍 Pressing；
* 不重置 explodeAt。

调试模式仅测试构建可用，正式构建必须关闭。

## 固定测试脚本 E：前后台

按 TASK-015 执行。

## 必交

* `TASK016_e2e_round1.mp4`
* `TASK016_short_press.mp4`
* `TASK016_slide_cancel.mp4`
* `TASK016_fake_press.mp4`
* `TASK016_background.mp4`
* 完整状态日志。

## 通过门槛

所有脚本 100% PASS 才能进入 UI 精修验收。不得用“多数情况没问题”通过。
