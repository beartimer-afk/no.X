# 12｜TASK-009：Explosion 完整毫秒级动画

## 目标

替换 TASK-006 的 `BOOM_TEST`，实现正式 Explosion。Explosion 是 v0.1 最高优先级动效，不允许简化成普通淡入淡出。

## 依赖

TASK-008 PASS。

## 触发时冻结

一旦满足真实爆炸条件：

1. 立即记录当前 holder 为 `loserIndex`；
2. `state = EXPLOSION`；
3. 取消所有未触发 Fake 定时；
4. 取消 Handoff 后续状态切换；
5. 忽略后续所有触摸事件；
6. 当前 Prompt 不再切换。

## Explosion 固定总时间轴：1180ms

### Phase E1：0–70ms / Collapse

* Core 从当前 scale → 0.90；
* 亮度快速提高；
* 背景不变；
* 不显示文字。

### Phase E2：70–210ms / Flash

* Core 从 0.90 → 1.18；
* 中心产生 `COLOR_WHITE_FLASH` 但只持续 ≤70ms；
* 外环断裂；
* 触发 `HAPTIC_BURST`；
* 触发正式 Explosion 音效。

### Phase E3：210–430ms / Break

* Core 主体快速透明到 0；
* 8–16 个抽象粉紫碎光向外扩散；
* 碎光必须是几何/光粒，不得是卡通炸弹碎片；
* 页面轻微震荡，幅度小；
* Prompt Card 在 210–350ms 透明到 0。

### Phase E4：430–760ms / Vacuum

* 屏幕接近纯 `COLOR_BG`；
* 不显示 Core；
* 不显示 Prompt；
* 不显示按钮；
* 不显示“BOOM”；
* 这段真空必须存在，不得删除。

### Phase E5：760–1180ms / Afterglow

* 背景出现很弱暗紫雾；
* 中心可有极低亮度残留光；
* 仍不显示输家文字；
* 1180ms 结束后才导航/切换 `LOSE_REVEAL`。

## 性能

* Explosion 期间不得做同步文件 IO；
* 不得生成大图；
* 粒子数量固定上限 16；
* 目标保持流畅；
* 如果设备性能不足，优先减少粒子数量，禁止删真空阶段。

## 禁止

* 卡通 BOOM 文字；
* 烟花；
* 彩纸；
* emoji；
* 全屏白闪超过 70ms；
* Explosion 时同时出现 `是你。`；
* 把 1180ms 压成 300ms。

## 必交

* `TASK009_explosion_60fps.mp4`
* 从视频抽帧：`E1_050ms.png`、`E2_150ms.png`、`E3_320ms.png`、`E4_600ms.png`、`E5_950ms.png`。

## 验收

A01 五段完整；A02 真空存在；A03 输家文字不提前；A04 无卡通元素；A05 不掉帧到明显肉眼卡顿；A06 1180ms 后准确进入 Lose Reveal。
