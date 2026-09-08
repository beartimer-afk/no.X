# 08｜TASK-005：合法松手 → PASS/Handoff → 下一位

## 目标

在 TASK-004 基础上实现：合法按压松手后进入 `BOMB_HANDOFF`，暂停 1300ms，然后切换 holder 并回到 `BOMB_WAITING_PRESS`。

本任务仍**不实现爆炸随机**。

## 依赖

TASK-004 PASS。

## 允许修改

```
pages/BombPage.ets
components/NoxBombCore.ets
model/GameSession.ets
constants/GameConstants.ets
services/HapticService.ets
```

## 固定常量

```
MIN_VALID_PRESS_MS = 1100
HANDOFF_PAUSE_MS = 1300
```

## 合法松手后的唯一顺序

当 `heldMs >= 1100` 且 ACTION\_UP：

1. 记录 `VALID_PRESS_RELEASED`；
2. `state = BOMB_HANDOFF`；
3. 触发 `HAPTIC_LOCK` 一次；
4. Core 亮度在 120ms 内快速收束；
5. 页面主提示从 `按住它，说出来，别松手。` 切换为 `传给对方。`；
6. Prompt Card 透明度降到 40%；
7. 禁止再次按 Core；
8. 等待 1300ms；
9. `holderIndex = holderIndex == 0 ? 1 : 0`；
10. 清空 `pressStartAt`；
11. `isPressValid = false`；
12. `state = BOMB_WAITING_PRESS`；
13. Prompt Card 恢复 100%；
14. 主提示恢复 `按住它，说出来，别松手。`。

## Handoff 页面不得新增独立页面

Handoff 是 BombPage 内状态。

## Handoff 视觉

* Core 缩放到 96%，不消失；
* 主外环亮度降低；
* 中心符号保持可见；
* `传给对方。` 使用 26fp；
* 下方附属文字 13fp：`别停太久。`
* 禁止全屏动画；
* 禁止箭头飞来飞去；
* 禁止出现人物头像。

## Prompt 处理

v0.1 每次 Handoff 完成后切换到下一条本地 Prompt。TASK-005 暂时只在两条测试 Prompt 间循环：

1. `说出你最想被亲吻的地方。`
2. `说一句你平时不太敢说的话。`

顺序固定循环，禁止随机。

## 禁止

* 禁止在 Handoff 期间重新开始按压；
* 禁止 Handoff 少于 1300ms；
* 禁止用按钮要求用户点击“已传递”；
* 禁止显示 Player 1 / Player 2；
* 禁止保存玩家身份。

## 必交

* `TASK005_valid_press_handoff.mp4`：完整按住→松手→传递→下一 Prompt；
* `TASK005_handoff_frame.png`：处于 Handoff 中间态截图。

## 验收

A01 合法松手必进 Handoff；A02 1300ms；A03 Handoff 不可按；A04 holder 正确切换；A05 Prompt 正确切换；A06 没有额外确认按钮；A07 连续做 10 次不乱状态。
