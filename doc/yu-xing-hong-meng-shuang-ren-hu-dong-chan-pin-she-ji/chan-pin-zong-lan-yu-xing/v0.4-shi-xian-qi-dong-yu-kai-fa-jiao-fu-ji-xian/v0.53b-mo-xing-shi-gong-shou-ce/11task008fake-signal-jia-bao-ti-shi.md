# 11｜TASK-008：Fake Signal 假爆提示

## 目标

加入最多两次 Fake Signal，制造“是不是要炸”的瞬时错觉，但绝不能改变真实爆炸时刻。

## 依赖

TASK-007 PASS。

## 固定规则

```
MAX_FAKE_SIGNAL = 2
```

每轮在生成真实 bombExplodeAt 后，再生成两个候选 Fake Signal 时间点。

### 允许时间范围

Fake 只能落在本轮总时长的：

```
35%–60%
60%–82%
```

每个区间最多 1 次。

若真实总时长过短导致候选时刻与真实爆炸相距 < 1200ms，则取消该 Fake，不补抽。

## Fake 触发条件

触发时必须满足：

* 当前 state 为 `BOMB_WAITING_PRESS` 或 `BOMB_PRESSING` 或视觉 Tense；
* 未处于 Handoff；
* 未爆炸；
* 该 Fake 未触发过。

## Fake Signal 固定表现，总时长 260ms

```
0–50ms: Core 快速收缩到 96%
50–110ms: HAPTIC_FAKE 一次 + 外环白粉闪亮
110–180ms: 轻微屏幕/核心抖动
180–260ms: 回到触发前视觉状态
```

声音：一个短促高频提示，不得使用真正 Explosion 音效。

## Fake 发生在 Pressing 中

用户必须仍保持按住；Fake 不得：

* 取消 pressStartAt；
* 将 isPressValid 重置；
* 误判 ACTION\_UP；
* 阻断触控事件。

## Fake 发生在 Waiting 中

只做视觉/触觉；不自动进入 Pressing。

## 禁止

* Fake 后重抽真实爆炸；
* 一轮超过 2 次；
* Fake 与真实爆炸音效相同；
* Fake 弹文案“差点爆炸”；
* Fake 产生弹窗；
* Fake 修改 Prompt。

## 必交

* `TASK008_fake_waiting.mp4`
* `TASK008_fake_pressing.mp4`
* 一轮日志：真实 explodeAt + fake1 + fake2。

## 验收

A01 ≤2 次；A02 不改真实时间；A03 Pressing 不被打断；A04 与真实爆炸能区分；A05 260ms 左右结束；A06 不出现提示弹窗。
