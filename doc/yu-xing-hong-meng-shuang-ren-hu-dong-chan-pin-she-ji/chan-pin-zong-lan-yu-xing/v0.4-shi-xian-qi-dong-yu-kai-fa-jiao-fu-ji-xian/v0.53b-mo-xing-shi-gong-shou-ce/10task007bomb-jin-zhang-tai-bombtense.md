# 10｜TASK-007：Bomb 紧张态 BOMB\_TENSE

## 目标

根据本轮已过去时间，将 BombPage 从正常视觉逐步提升到紧张视觉。不得影响爆炸时间。

## 依赖

TASK-006 PASS。

## 紧张度计算

仅用于视觉：

```
total = bombExplodeAt - bombStartAt
elapsed = now - bombStartAt
ratio = clamp(elapsed / total, 0, 1)
```

由于 explodeAt 对用户不可见，ratio 只能内部使用。

## 视觉阶段

### Calm：ratio < 0.45

* `state` 可保持 WAITING/PRESSING；
* Core 基础粉紫光；
* 呼吸周期约 1800ms；
* 无电裂纹；
* 无危险文字。

### Uneasy：0.45 ≤ ratio < 0.72

* 外环亮度 +15%；
* 呼吸周期 1200ms；
* 背景出现非常弱紫色雾化；
* Prompt Card 不变；
* 不出现红色警告。

### Tense：ratio ≥ 0.72

逻辑标志 `visualTense = true`，页面表现：

* Core 外环粉色更亮；
* 加入 `COLOR_DANGER` 只用于少量高光；
* 呼吸周期 720ms；
* 外环允许 1–2px 视觉抖动；
* 页面标题从 `按住并回答` 改为 `继续按住…`；
* 次文案 `越来越热了。`；
* 允许非常弱的波形装饰；
* 禁止改变 Prompt 内容。

## 关键原则

用户不应能从视觉准确推算爆炸时刻。因此：

* Tense 进入后并不表示马上爆；
* 不显示百分比；
* 不显示剩余秒；
* 不使用线性越来越满的进度环；
* Core 外环可增强但不得形成可计算进度。

## 按压逻辑

Tense 仍使用 TASK-004 相同按压规则，1100ms 不变。不得因为紧张态增加按压时长。

## 必交

* `TASK007_calm.png`
* `TASK007_uneasy.png`
* `TASK007_tense.png`
* `TASK007_tension_transition.mp4`

## 验收

A01 三阶段能明显区分；A02 无可计算进度；A03 Tense 不改真实 explodeAt；A04 Prompt 不变；A05 仍能正常按住/Handoff。
