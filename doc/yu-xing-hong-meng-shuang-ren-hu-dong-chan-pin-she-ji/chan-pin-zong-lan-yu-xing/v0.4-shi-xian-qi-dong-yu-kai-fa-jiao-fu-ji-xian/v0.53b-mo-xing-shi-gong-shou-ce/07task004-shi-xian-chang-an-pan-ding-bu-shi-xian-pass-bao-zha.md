# 07｜TASK-004：实现长按判定，不实现 PASS/爆炸

## 目标

只实现 Core 的按下、持续、松开判定，让页面能从 `BOMB_WAITING_PRESS` 进入 `BOMB_PRESSING`，并判断按压是否达到 1100ms。

**本任务禁止进入 Handoff，禁止爆炸，禁止随机。**

## 依赖

TASK-003 PASS。

## 允许修改

```
pages/BombPage.ets
components/NoxBombCore.ets
model/GameSession.ets
model/GameState.ets
constants/GameConstants.ets
utils/TimeUtils.ets
```

## 固定常量

```
MIN_VALID_PRESS_MS = 1100
CORE_VISUAL_DIAMETER = 156vp
CORE_HIT_DIAMETER = 188vp
```

## 触摸事件规则

### ACTION\_DOWN

只有事件坐标在 188vp 命中圆内时合法。

依次执行：

1. 如果当前状态不是 `BOMB_WAITING_PRESS`，忽略；
2. `pressStartAt = monotonicNowMs()`；
3. `isPressValid = false`；
4. `state = BOMB_PRESSING`；
5. 触发一次 `HAPTIC_TAP` 占位调用；
6. Core 进入 Pressing 视觉态。

禁止在 DOWN 时启动 1100ms 动画计时器来代表真实按压时长；真实判定必须使用 monotonic 时间差。

### 持续按住

每帧/定时刷新只用于视觉；合法性计算：

```
heldMs = now - pressStartAt
if heldMs >= 1100:
    isPressValid = true
```

合法后不得自动松手、不得自动 PASS。

### ACTION\_UP / CANCEL

计算：

```
heldMs = upAt - pressStartAt
```

如果 `< 1100ms`：

* `isPressValid = false`
* `state = BOMB_WAITING_PRESS`
* Core 回到 Waiting 视觉态
* 页面底部/近 Core 出现 900ms 临时提示：`再按久一点。`
* 不进入下一状态

如果 `>= 1100ms`：

* 本任务只记录日志 `VALID_PRESS_RELEASED`
* `state` 临时回 `BOMB_WAITING_PRESS`
* 不进入 Handoff

## Pressing 视觉态

按下后：

* Core 视觉直径仍为 156vp；
* 不缩到 140vp 以下；
* 中心亮度提高；
* 内环从 100% scale 到 94%，时长 160ms；
* 外环发光提高；
* 不出现数字倒计时；
* 不出现线性进度条。

合法阈值达到时：

* 只做一次非常轻的 `Lock` 反馈占位；
* 外环亮度提升一个层级；
* 不显示“完成”“100%”。

## 触控边界

* 手指按下在圆内，随后移动到命中区外超过 12vp：视为 CANCEL；
* CANCEL 等同短按失败；
* 多指：只接受第一根手指；第二根手指完全忽略；
* 处于 Pressing 时重复 DOWN 忽略。

## 禁止

* 禁止松手后换玩家；
* 禁止 PASS 字样；
* 禁止真实炸弹计时；
* 禁止爆炸；
* 禁止把按压做成按钮点击；
* 禁止语音识别。

## 必交素材

* `TASK004_press_short.mp4`：按 0.5 秒后松手；
* `TASK004_press_valid.mp4`：按约 1.5 秒后松手；
* `TASK004_press_move_cancel.mp4`：按住后滑出。

## 验收

A01 0.5s 不合法；A02 1.5s 合法；A03 滑出取消；A04 无进度数字；A05 没有自动 PASS；A06 按压期间 Core 明显响应；A07 多指不崩溃。
