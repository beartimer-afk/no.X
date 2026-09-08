# 17｜TASK-014：触觉与声音映射，不得自由配音效

## 目标

为已经存在的状态事件接入统一 Haptic/Sound Service。开发模型不得自行决定“哪里震一下更爽”。

## 依赖

TASK-013 PASS。

## 唯一触觉事件

```
HAPTIC_TAP
HAPTIC_LOCK
HAPTIC_FAKE
HAPTIC_BURST
```

## 映射

### HAPTIC\_TAP

触发：

* Home `开始游戏`；
* Lose `查看挑战`；
* Challenge `去完成`；
* Bomb Core ACTION\_DOWN。

要求：短、轻、单次。

### HAPTIC\_LOCK

触发：

* 按压达到 1100ms 合法阈值；
* 合法松手进入 Handoff 时**不要再重复第二次**。如果平台 API 难以区分，优先只在阈值达成时触发。

要求：比 TAP 稍实，但不强。

### HAPTIC\_FAKE

触发：Fake Signal 50–110ms 区段一次。

要求：短而突兀，但明显弱于 Burst。

### HAPTIC\_BURST

触发：Explosion E2 70–210ms 中一次。

要求：全 App 最强触觉。

## 唯一声音事件

```
SOUND_PRESS_AMBIENT
SOUND_FAKE
SOUND_EXPLOSION
SOUND_REVEAL
```

### SOUND\_PRESS\_AMBIENT

* 不是循环音乐；
* 是低音量电流/脉冲氛围；
* 只在 BombPage；
* Handoff 可轻微降低音量；
* 离开 BombPage 必须停止。

### SOUND\_FAKE

* 短促高频 glitch；
* 不能像 Explosion。

### SOUND\_EXPLOSION

* 在 E2 同步；
* 峰值只有一次；
* 不要长尾超过约 700ms。

### SOUND\_REVEAL

* Lose 标题出现时非常轻的低频/氛围提示；
* 不得抢 Explosion。

## 设置

v0.1 暂不做独立声音设置页。跟随系统媒体音量。

## 禁止

* 背景音乐；
* 每个按钮都震；
* 列表滑动震；
* 随机音效；
* 使用儿童游戏 BOOM 音；
* 使用人声呻吟等素材。

## 必交

一段从按下到 Explosion→Lose 的真机录屏，并现场说明触觉节点（视频无法记录触觉时附日志）。

## 验收

A01 只有四类触觉；A02 Burst 最强；A03 Fake 明显不同；A04 Bomb 离开后 Ambient 停止；A05 无背景音乐。
