# 13｜触觉与声音时间轴

## 原则

触觉比声音更重要，因为这个产品天然是“手机在手里”的游戏。声音默认可关闭，但触觉应该让用户感到 Core 是一个真实物体。

## Haptic Vocabulary

只定义四种语义，禁止开发随意新增：

### Tap

很轻，30–45ms。

用途：选择、轻按钮。

### Lock

中等，55–80ms。

用途：长按完成、Secret Match 解锁、卡片吸附。

### Fake

短促但略不规则，70–100ms，可使用双段。

用途：Bomb Fake Signal。

### Burst

最强，分两段：第一击 + 120–180ms 后第二击。

用途：Explosion。

## Bomb 时间轴示例

### Touch Core

0ms：无 haptic。

250ms：Tap。

800ms Hold 完成：Lock。

### Fake Signal

0ms：Fake 第一段； 60–110ms：可选第二段； 视觉在同一时间窗口发生收缩和亮度脉冲。

### Explosion

0ms：Burst 强第一击； 140ms：第二击； 220ms 后不再连续震动，给真实世界反应留空间。

## 声音风格

不要使用：

* 卡通炸弹“砰”；
* 倒计时滴答；
* 赌场 Jackpot；
* 电子 EDM 音效包。

推荐材质：

* 低频空气压力；
* 玻璃 / 金属共振的抽象版本；
* 极短低频冲击；
* 柔和磁吸 click。

## 声音层级

1. UI Click：极弱；
2. Core Ambient：可选、低存在感；
3. Fake Pulse：低频短声；
4. Explosion Peak：全局最强，但短；
5. Aftermath：瞬间静音比拖尾更重要。

## 音量

App 内不提供复杂混音台。Settings 只需要：声音 On / Off。

遵循系统媒体音量，不尝试绕过静音 / 勿扰逻辑。

## 同步标准

视觉、触觉、声音三者关键事件最大允许感知偏差目标 < 50ms。Explosion 必须重点真机校验。

## 验收

关掉屏幕声音只靠触觉，仍应能分辨：普通操作、Fake、真正 Explosion。

关掉触觉只靠视觉，仍应完整可玩。
