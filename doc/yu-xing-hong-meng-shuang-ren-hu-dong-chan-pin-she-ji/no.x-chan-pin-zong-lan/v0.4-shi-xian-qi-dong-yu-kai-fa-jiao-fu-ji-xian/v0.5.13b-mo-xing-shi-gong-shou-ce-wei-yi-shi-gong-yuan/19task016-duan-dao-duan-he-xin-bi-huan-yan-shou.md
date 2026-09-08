# 19｜TASK-016：端到端核心闭环验收

## 目标

本任务原则上不新增功能，只验证此前任务组合后的完整链路。

## 依赖

TASK-001 至 TASK-015 全 PASS。

## 脚本 A：正常完整一轮

冷启动 → Home → 开始游戏 → Round Ready → 我准备好了 → Bomb Waiting → 按住 >1100ms → 合法松手 → Handoff → 下一人按住 → 至少 2 次 Handoff → 保持某次按压直到自然 Explosion → 1180ms Explosion → Lose → 等 1100ms → 查看挑战 → Challenge → 去完成 → 下一轮 → ROUND 02。

Explosion 发生时必须能看到当前手指仍处于 Core Pressing。

## 脚本 B：短按

按 300–700ms 松手；回 Waiting；显示 再按久一点。；holder 不变；bombTargetHoldMs 不变；bombAccumulatedHoldMs 必须增加本次实际 heldMs。

## 脚本 C：滑出取消

按住后滑出命中区超过 12vp；holder 不变；不 Handoff；target 不变；已真实按住的时间计入 accumulated。

## 脚本 D：Fake Pressing

Debug 强制在 Pressing 跨过 Fake threshold；手指保持；按压累计继续；Fake 后仍 Pressing；target 不变。

## 脚本 E：Handoff 暂停

记录 Handoff 前 accumulated；1300ms Handoff 内数值必须完全不增长。

## 脚本 F：Waiting 暂停

Handoff 完成后故意等待 5 秒不按；accumulated 不增长；不得 Explosion。

## 脚本 G：前后台

按 TASK-015。

## 通过门槛

所有脚本 100% PASS 才进入最终 UI 精修。不得用“多数情况正常”通过。
