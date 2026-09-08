# 08｜TASK-005：合法松手 → PASS/Handoff → 下一位

## 目标

在 TASK-004 基础上实现：合法按压松手后进入 BOMB\_HANDOFF，暂停 1300ms，然后切换 holder 并回到 BOMB\_WAITING\_PRESS。本任务仍不实现 Bomb 随机预算/Explosion。

## 依赖

TASK-004 PASS。

## 允许修改

pages/BombPage.ets components/NoxBombCore.ets model/GameSession.ets constants/GameConstants.ets services/HapticService.ets

## 固定常量

MIN\_VALID\_PRESS\_MS = 1100 HANDOFF\_PAUSE\_MS = 1300

## 合法松手后的唯一顺序

当 heldMs >=1100 且 ACTION\_UP：

1. 记录 VALID\_PRESS\_RELEASED；
2. state = BOMB\_HANDOFF；
3. 触发 HAPTIC\_TAP 一次；此处禁止再次触发 HAPTIC\_LOCK；
4. Core 亮度 120ms 内收束；
5. 主提示切换为 传给对方。；
6. Prompt Card opacity 降到 40%；
7. 禁止再次按 Core；
8. 等待 1300ms；
9. holderIndex = holderIndex == 0 ? 1 : 0；
10. 清空 pressStartAt / pressSafeUntil；
11. isPressValid = false；
12. state = BOMB\_WAITING\_PRESS；
13. Prompt Card 恢复 100%；
14. 主提示恢复 按住它，说出来，别松手。。

## Handoff 视觉

Core scale=0.96；外环亮度降低；中心符号保持可见；传给对方。 使用 26fp；下方 13fp：别停太久。。禁止按钮、头像、箭头飞行动画。

## Prompt 处理

TASK-005 暂时只在两条测试 Prompt 固定循环：

1. 说出你最想被亲吻的地方。
2. 说一句你平时不太敢说的话。

禁止随机。

## 禁止

Handoff 期间重新按压；Handoff 少于 1300ms；点击“已传递”按钮；Player 1/2；合法松手再次 HAPTIC\_LOCK。

## 必交

TASK005\_valid\_press\_handoff.mp4 TASK005\_handoff\_frame.png

## 验收

A01 合法松手必进 Handoff；A02 1300ms；A03 Handoff 不可按；A04 holder 切换；A05 Prompt 切换；A06 无额外按钮；A07 连续 10 次不乱状态；A08 Lock 只在达到 1100ms 阈值时发生一次。
