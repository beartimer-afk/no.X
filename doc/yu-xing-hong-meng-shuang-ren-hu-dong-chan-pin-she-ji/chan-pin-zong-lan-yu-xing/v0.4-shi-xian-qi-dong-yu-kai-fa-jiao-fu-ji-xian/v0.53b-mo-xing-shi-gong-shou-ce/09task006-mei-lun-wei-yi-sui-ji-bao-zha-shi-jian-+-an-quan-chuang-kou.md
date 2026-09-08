# 09｜TASK-006：每轮唯一随机爆炸时间 + 安全窗口

## 目标

加入真正的 Bomb 时钟，但先不做复杂 Explosion 动画。到时只切换为一个测试 `EXPLOSION` 黑屏标记。

## 依赖

TASK-005 PASS。

## 允许修改

```
pages/RoundReadyPage.ets
pages/BombPage.ets
model/GameSession.ets
constants/GameConstants.ets
services/RandomService.ets
utils/TimeUtils.ets
```

## 固定常量

```
BOMB_MIN_DURATION_MS = 14000
BOMB_MAX_DURATION_MS = 30000
NEW_HOLDER_SAFE_MS = 400
```

## 随机函数要求

`RandomService.randomIntInclusive(min, max)` 返回包含上下界的整数。

每轮只在点击 `我准备好了`、准备进入 Bomb 时执行一次：

```
delay = randomIntInclusive(14000, 30000)
bombStartAt = monotonicNowMs()
bombExplodeAt = bombStartAt + delay
```

不得在以下行为重新计算：

* 按下；
* 松手；
* Handoff；
* Prompt 切换；
* Fake Signal；
* 前后台恢复（后续任务另有定义）。

## 检查爆炸方式

BombPage 运行期间使用可靠的时间检查方式，比较 `monotonicNowMs() >= bombExplodeAt`。

不得通过每 1 秒减一个整数实现；不得让帧率误差累计成剩余时间。

## 新持有者安全窗口

每次 Handoff 完成后记录：

```
holderActivatedAt = now
```

若真实 `bombExplodeAt` 落在新 holder 激活后的前 400ms 内：

* 不立即爆；
* 将实际触发延迟到 `holderActivatedAt + 400ms`；
* 这不是重新随机；只是最低可感知安全窗口。

## EXPLOSION 测试状态

本任务到时后：

* `state = EXPLOSION`；
* 屏幕立即背景变 `COLOR_BG`；
* 中央显示测试文字 `BOOM_TEST`；
* 停止接受触控；
* 不导航 LoseReveal。

## 禁止

* UI 显示 14–30 秒范围；
* 显示剩余秒数；
* 根据 Handoff 次数修改爆炸时间；
* 每次 PASS 后重抽；
* “为了公平”偏向某一 holder。

## 必交

* 运行 20 轮的日志文本，列出每轮生成 delay；
* `TASK006_random_explosion.mp4`：至少一轮自然触发测试爆炸。

## 验收

A01 delay 全在 14000–30000；A02 一轮只生成一次；A03 PASS 不改变 explodeAt；A04 400ms 安全窗有效；A05 到时停止触控；A06 UI 不泄露剩余时间。
