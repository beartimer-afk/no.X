# 18｜TASK-015：前后台、中断与异常恢复

## 目标

规定切后台、锁屏、来电中断的唯一行为。由于 Bomb 只累计真实按压时间，后台天然暂停，不存在 wall-clock deadline 修正。

## 核心原则

后台不累计 Bomb 预算，后台绝不 Explosion。

## 进入后台

若 state = BOMB\_PRESSING：

1. now = monotonicNowMs()；
2. heldMs = now - pressStartAt；
3. bombAccumulatedHoldMs += max(heldMs,0)；
4. pressStartAt = null；pressSafeUntil = null；isPressValid=false；
5. state = BOMB\_WAITING\_PRESS；
6. 停止 Ambient；
7. Fake 动效如正在播放立即停止并恢复 Waiting 视觉。

若 state = BOMB\_WAITING\_PRESS：只停止 Ambient，累计值不变。

若 state = BOMB\_HANDOFF：取消剩余 Handoff timer；holderIndex 不执行尚未发生的切换；恢复状态设为 BOMB\_WAITING\_PRESS；累计值不变。

## 返回前台

若之前在 Bomb：

1. bombTargetHoldMs 不变；
2. bombAccumulatedHoldMs 不变；
3. state = BOMB\_WAITING\_PRESS；
4. visualPhase 按 accumulated/target 重算；
5. 恢复 Ambient；
6. 显示 900ms 弱提示：继续。；
7. 下一次 CORE\_DOWN 自动获得新的 400ms press safety。

禁止额外赠送 2.5s，也禁止重抽 target。

## Fake

已触发 Fake 保持已触发；未触发 Fake 阈值保持原值。前后台后禁止重新随机 Fake。

## Explosion 中后台

E1–E5 期间进后台：恢复后直接 LOSE\_REVEAL；不重播 Explosion；loserIndex 必须保留。

## Lose / Challenge

恢复原页面原状态。

## 进程被杀

v0.1 不恢复 Session；冷启动 Home。

## 禁止

后台累计、后台爆炸、恢复 Pressing、重抽 target、清零 accumulated、恢复后直接 Explosion。

## 必交

TASK015\_background\_pressing.mp4 TASK015\_background\_waiting.mp4 TASK015\_background\_handoff.mp4 TASK015\_background\_explosion.mp4

## 验收

A01 后台不累计；A02 Pressing 已产生的按压时间正确结算；A03 target 不变；A04 Handoff 中断不偷偷切 holder；A05 Explosion 中断后到 Lose；A06 杀进程回 Home。
