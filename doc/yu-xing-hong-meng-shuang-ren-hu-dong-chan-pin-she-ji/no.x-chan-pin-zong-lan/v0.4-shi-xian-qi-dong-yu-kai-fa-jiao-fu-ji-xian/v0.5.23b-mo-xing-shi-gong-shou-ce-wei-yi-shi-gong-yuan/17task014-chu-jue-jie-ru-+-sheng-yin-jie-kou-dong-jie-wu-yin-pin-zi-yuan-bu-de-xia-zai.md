# 17｜TASK-014：触觉接入 + 声音接口冻结（无音频资源不得下载）

## 目标

为既有状态事件接入统一 HapticService；SoundService 在没有正式设计侧音频资源时保持**可调用但静默**。开发模型不得自行寻找或生成音效。

## 依赖

TASK-013 PASS。

## 唯一触觉事件

HAPTIC\_TAP HAPTIC\_LOCK HAPTIC\_FAKE HAPTIC\_BURST

### HAPTIC\_TAP

只允许：Home 开始游戏；Bomb Core ACTION\_DOWN；合法 ACTION\_UP 进入 Handoff；Lose 查看挑战；Challenge 去完成。短、轻、单次。

### HAPTIC\_LOCK

只允许按压首次达到1100ms合法阈值时触发一次。合法松手禁止再次 Lock。

### HAPTIC\_FAKE

Fake 50–110ms 区段一次，明显弱于 Burst。

### HAPTIC\_BURST

Explosion E2 一次，全 App 最强。

## 声音资源 Gate

施工前检查设计侧资源包及仓库是否存在**人工明确提供的正式音频资源清单**。

v0.5.2 的 `NOX_static_assets_v0.1` **没有音频文件**，因此本 TASK 默认执行静默路径：

* SoundService 必须保留事件接口；
* 每个事件输出固定 Debug 日志；
* Release 可以 no-op；
* 不新增 wav/mp3/ogg；
* 不联网下载；
* 不用系统随机提示音代替。

只有未来设计侧发布新的正式音频资源包并更新本 TASK 后，才允许真正播放音频。

## 唯一声音事件名

SOUND\_PRESS\_AMBIENT SOUND\_FAKE SOUND\_EXPLOSION SOUND\_REVEAL

当前 v0.1 行为：调用接口 + Debug 日志，不播放素材。

日志示例：`[NOX][SOUND] SOUND_EXPLOSION asset=NONE noop=true`

## 禁止

背景音乐、每个按钮都震、随机音效、儿童 BOOM、人声呻吟、免费音效网站下载、AI 生成音效、用系统通知音替代。

## 必交

* 触觉节点真机/模拟日志；
* `TASK014_sound_noop_log.txt`，证明四种声音事件均能被调用且无外部音频资源；
* 本 TASK 新增文件列表，必须不含音频文件。

## 验收

A01 只有四类触觉；A02 Lock一次；A03合法 release 使用Tap；A04 Burst最强；A05 SoundService四事件可调用；A06 新增音频文件=0；A07 无网络下载/第三方音频依赖。
