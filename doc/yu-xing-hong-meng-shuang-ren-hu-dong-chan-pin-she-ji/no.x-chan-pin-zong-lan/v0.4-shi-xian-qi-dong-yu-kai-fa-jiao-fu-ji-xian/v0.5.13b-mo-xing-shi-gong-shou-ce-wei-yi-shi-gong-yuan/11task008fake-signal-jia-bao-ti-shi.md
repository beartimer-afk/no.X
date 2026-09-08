# 11｜TASK-008：Fake Signal 假爆提示

## 目标

加入最多两次 Fake Signal。Fake 只在用户正在按住 Core 时触发，强化“边按、边说、边担心爆炸”的压力。

## 依赖

TASK-007 PASS。

## 固定字段

Fake 状态必须写入 GameSession：fake1ThresholdMs、fake2ThresholdMs、fake1Triggered、fake2Triggered、fakeSignalCount。禁止只存在 BombPage 局部变量。

## 初始化

根据 bombTargetHoldMs 生成两个累计按压阈值：fake1ThresholdMs 在 target 35%–60% 内；fake2ThresholdMs 在 60%–82% 内。每区间最多一次。

若某阈值与真实 target 差 <1200ms，则该字段设为 null，不补抽。

初始化时：fake1Triggered=false；fake2Triggered=false；fakeSignalCount=0。

## 唯一触发条件

仅允许 state == BOMB\_PRESSING，且对应 threshold 非 null、尚未 triggered、effectiveHoldMs >= threshold、尚未满足真实 Explosion 条件。

触发后立即先把对应 Triggered=true、fakeSignalCount +=1，再播放 260ms Fake。这样即使动画被后台打断也不会重复触发。

如果同一次检查同时满足 Fake 与真实 Explosion，真实 Explosion 优先，Fake 取消且不计入 fakeSignalCount。

Waiting/Handoff 不触发 Fake。

## 固定表现 260ms

0–50ms Core scale→0.96；50–110ms HAPTIC\_FAKE + 外环白粉闪；110–180ms 轻微抖动；180–260ms 恢复触发前视觉。

声音：短促高频 glitch，不得使用 Explosion 音效。

## Fake 不得打断 Pressing

不得清 pressStartAt、修改 accumulated、重置 isPressValid、阻断 ACTION\_UP/CANCEL、改变 target、修改 Prompt。

## 必交

TASK008\_fake\_pressing.mp4；一轮日志 target / 两个 threshold / triggered / 实际触发 accumulated。

## 验收

A01 ≤2；A02 只在 Pressing；A03 不打断；A04 Explosion 优先；A05 260ms；A06 前后台后不重复 Fake。
