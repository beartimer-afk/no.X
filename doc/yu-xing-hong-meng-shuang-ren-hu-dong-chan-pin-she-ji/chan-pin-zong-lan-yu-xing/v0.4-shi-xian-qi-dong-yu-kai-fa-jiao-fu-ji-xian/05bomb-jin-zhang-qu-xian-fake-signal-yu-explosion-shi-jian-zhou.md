# 05｜Bomb 紧张曲线、Fake Signal 与 Explosion 时间轴

## 紧张度不是可见进度

内部变量：`p = accumulatedHold / deadline`。

用户不可看到 p。

视觉只按区间变化：

* Calm：`0.00 ≤ p < 0.38`
* Warm：`0.38 ≤ p < 0.62`
* Tense：`0.62 ≤ p < 0.82`
* Critical：`0.82 ≤ p < 1.00`

## 各阶段视觉固定值

| 阶段       | Core Scale 基准 | Glow | Ring 抖动 | 背景亮度 |
| -------- | ------------: | ---: | ------: | ---: |
| Calm     |          1.00 | 0.72 |       0 | 1.00 |
| Warm     |          1.01 | 0.86 |  ±0.5vp | 1.02 |
| Tense    |          1.02 | 1.00 |  ±1.2vp | 1.04 |
| Critical |         1.035 | 1.18 |  ±2.0vp | 1.06 |

抖动只作用于 Pressure Ring，不允许整屏抖。

## Fake Signal

每 Round 固定最多 2 次：

* Fake #1 目标点：`p=0.46 ± random(0.03)`；
* Fake #2 目标点：`p=0.72 ± random(0.03)`；
* 如目标点发生在 Handoff Overlay，则推迟到下一次按下后的 650ms 之后；
* 距离真实爆炸 <1.5s 时取消尚未执行的 Fake。

### Fake #1

总长 180ms：

* 0–50ms：Core 白粉闪 1 次；
* 50ms：触觉 `FakeMedium`；
* 50–140ms：Pressure Ring 半径 +6vp；
* 140–180ms：恢复；
* 不播放大音效，只播放极短电流 Tick。

### Fake #2

总长 320ms：

* 0–70ms：Ring 出现 3 段断裂；
* 70ms：双触觉 `FakeDouble`；
* 70–140ms：Glow 强度 ×1.55；
* 140–220ms：页面出现一次 8% opacity 的粉色全屏 Flash；
* 220–320ms：恢复；
* 声音：短促升调脉冲，不得像真正爆炸。

## Critical 文案变化

进入 `p>=0.82` 后：

主标题由 `按住并回答` 淡变为：

`继续按住…`

副提示固定：`越来越热了。`

持续 180ms crossfade；不得突然跳字。

## Explosion 触发条件

当 `accumulatedHold >= deadline` 且当前手指仍处于有效按压状态时立即触发。

真实 Explosion 必须取消当前所有 Fake 动画和循环动画。

## Explosion 时间轴

### 0–70ms｜收缩

* Core scale 1.03 → 0.82；
* Glow 暂时下降到 0.35；
* 音效前导低频 `thump`；
* 强触觉前导一次。

### 70–210ms｜释放

* Core scale 0.82 → 1.38；
* Inner Disk 亮度到纯白粉；
* Primary Ring 分裂成 8–12 个抽象弧段；
* 弧段向外扩散 32–58vp；
* 全屏 Flash 最高 opacity=0.28；
* Explosion 主音效；
* 触觉 `Burst`。

### 210–430ms｜碎散

* 弧段 opacity 1→0；
* 背景 Glow 扩散到 260vp；
* Prompt Card opacity 1→0；
* 标题 opacity 1→0。

### 430–760ms｜真空

* 页面只剩 `bg.base` + 极弱紫雾；
* 不显示文字；
* 不播放音效；
* 不震动。

### 760–1180ms｜Lose Reveal

* `是你。` opacity 0→1；
* Y 10→0；
* 28sp / 700；
* 背后可出现模糊成人摄影背景（仅当资源存在）；
* 没有资源时保持暗紫背景，不得自行生成。

## 性能

Explosion 必须在目标设备保持视觉连续；出现明显掉帧即视为 P0 UI Bug。禁止为了性能直接删除 Burst 层，优先减少粒子数量 / 使用预渲染资源。
