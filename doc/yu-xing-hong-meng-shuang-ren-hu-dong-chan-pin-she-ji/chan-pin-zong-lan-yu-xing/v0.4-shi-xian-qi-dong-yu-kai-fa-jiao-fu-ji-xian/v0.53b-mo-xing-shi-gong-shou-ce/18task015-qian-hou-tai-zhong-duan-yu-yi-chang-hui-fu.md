# 18｜TASK-015：前后台、中断与异常恢复

## 目标

规定用户切后台、锁屏、来电中断等情况下的唯一行为。不得让 3B 模型自己决定“继续计时还是暂停”。

## 依赖

TASK-014 PASS。

## 核心原则

**游戏进入后台时，Bomb 暂停。回到前台后继续剩余时间。**

不得后台偷偷爆炸。

## 进入后台时

如果当前 state 属于：

```
BOMB_WAITING_PRESS
BOMB_PRESSING
BOMB_HANDOFF
BOMB_TENSE
```

执行：

1. `backgroundAt = monotonicNowMs()`；
2. 记录 `remainingToExplosion = bombExplodeAt - backgroundAt`；
3. 停止 Fake Signal 调度；
4. 停止 Ambient sound；
5. 如果正在 Pressing：取消本次按压，`pressStartAt = null`，`isPressValid = false`；
6. 不改变 holderIndex。

其他页面只记录生命周期，无需特别处理。

## 返回前台时

若之前处于 Bomb：

1. 恢复到 `BOMB_WAITING_PRESS`；
2. `bombStartAt = now`（仅用于重新计算内部基准）；
3. `bombExplodeAt = now + max(remainingToExplosion, 2500ms)`；
4. 最低再给 2500ms，避免一回来立刻爆；
5. 清空旧 Fake 计划，根据新的 remaining 重新安排最多 1 次 Fake；
6. 恢复 Ambient；
7. 页面显示 900ms 轻提示：`继续。`。

## Handoff 中切后台

恢复后不继续剩余 Handoff；直接回 `BOMB_WAITING_PRESS`，holderIndex 保持后台前尚未完成切换的那个 holder。

## Explosion 中切后台

* 若 E1–E5 期间进入后台：恢复后直接进入 `LOSE_REVEAL`；
* 不重播 Explosion；
* loserIndex 必须保留。

## Lose / Challenge 切后台

恢复原页面原状态。

## 进程被杀

v0.1 不恢复未完成 Session。冷启动回 Home。不得实现复杂持久化。

## 禁止

* 后台计时继续；
* 回来直接 Explosion；
* 恢复 Pressing；
* 保存手指按压进度；
* 自动跳过 Lose 页。

## 必交

* `TASK015_background_pressing.mp4`
* `TASK015_background_waiting.mp4`
* `TASK015_background_explosion.mp4`

## 验收

A01 后台不爆；A02 Pressing 被取消；A03 回前台至少 2.5s 安全；A04 Explosion 中断后到 Lose；A05 进程杀死回 Home。
