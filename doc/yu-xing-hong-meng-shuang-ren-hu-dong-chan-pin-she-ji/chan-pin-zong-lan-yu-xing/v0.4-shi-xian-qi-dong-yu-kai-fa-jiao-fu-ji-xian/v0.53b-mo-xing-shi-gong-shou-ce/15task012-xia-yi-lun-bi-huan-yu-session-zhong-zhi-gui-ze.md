# 15｜TASK-012：下一轮闭环与 Session 重置规则

## 目标

让 `Challenge Reveal → 下一轮 → Round Ready → Bomb` 能连续循环至少 5 轮且不残留上一轮状态。

## 依赖

TASK-011 PASS。

## 点击 `下一轮` 的唯一重置顺序

1. `roundIndex += 1`；
2. `state = ROUND_TRANSITION`；
3. 清空 `pressStartAt`；
4. `isPressValid = false`；
5. `fakeSignalCount = 0`；
6. `currentChallengeId = null`；
7. 不清空全局 Challenge 历史（用于避免连续重复，后续数据任务定义）；
8. 等待 280ms 页面过渡；
9. 进入 `ROUND_READY`；
10. Round Ready 顶部显示新的 `ROUND N`；
11. 只有用户再次点击 `我准备好了` 才生成新的 bombStartAt / bombExplodeAt。

## holderIndex 规则

新一轮保持上一轮输家以外的另一人先拿手机：

```
holderIndex = loserIndex == 0 ? 1 : 0
```

如果 loserIndex 不存在（异常恢复），默认 `holderIndex = 0` 并记录日志，不允许崩溃。

## Round Transition 视觉

* 不建独立页面；
* Challenge 页面轻淡出 160ms；
* 背景保持 `COLOR_BG`；
* Round Ready 淡入 120ms；
* 总计约 280ms。

## 绝对禁止的状态残留

下一轮不得保留：

* 上一轮 Core 的 Tense 光效；
* 上一轮 Fake 已触发标志；
* 上一轮 Prompt；
* 上一轮 Challenge 完成态；
* 上一轮爆炸粒子；
* 上一轮按钮禁用计时。

## 必交

* `TASK012_five_rounds.mp4` 或分段录屏，证明连续 5 轮；
* 每轮状态日志。

## 验收

A01 roundIndex 1→5 正确；A02 每轮都需重新点准备；A03 每轮重新生成唯一 explodeAt；A04 无视觉残留；A05 holder 规则正确；A06 连续 5 轮无崩溃。
