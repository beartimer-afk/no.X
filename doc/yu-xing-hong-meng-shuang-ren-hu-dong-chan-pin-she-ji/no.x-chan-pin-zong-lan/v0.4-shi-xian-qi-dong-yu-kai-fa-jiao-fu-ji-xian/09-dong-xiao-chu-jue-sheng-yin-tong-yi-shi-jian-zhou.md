# 09｜动效、触觉、声音统一时间轴

## 动效原则

* 所有业务页面过渡 180–280ms；
* 不使用弹簧过冲超过 1.035；
* 不使用 Material shared-axis 默认动画；
* 不使用翻页、立方体、3D 卡片；
* Bomb 本身允许更激烈，但文字区必须稳定。

## 触觉事件

| 事件               | 名称           | 强度      |
| ---------------- | ------------ | ------- |
| 普通按钮 Tap         | `Tap`        | 轻       |
| Core pointerDown | `Lock`       | 中轻、短    |
| Early Release    | `Error`      | 中、单次    |
| Fake #1          | `FakeMedium` | 中       |
| Fake #2          | `FakeDouble` | 中 × 2   |
| Explosion        | `Burst`      | 强 + 短尾震 |

禁止每帧震动；Critical 阶段不做连续 vibration。

## 声音

首版只允许 5 类：

1. UI Tap：极弱；
2. Core Lock：低频 click；
3. Fake Tick：短电流；
4. Fake Pulse：升调脉冲；
5. Explosion：主爆音 + 低频冲击。

### 音量相对值

* Tap：0.18
* Lock：0.28
* Fake Tick：0.35
* Fake Pulse：0.46
* Explosion：0.78

不得加入背景音乐首版。

## 无声模式

系统媒体音量为 0 时不强制出声；触觉仍工作（如果系统允许）。

## 减少动态效果

若系统开启 Reduce Motion：

* 保留 opacity 变化；
* Core scale 最大变化降至 50%；
* Explosion 弧段减少 60%；
* 不删除 430–760ms 的“真空”；
* 触觉不自动关闭。
