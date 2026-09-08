# 17｜TASK-014：触觉与声音映射，不得自由配音效

## 目标

为既有状态事件接入统一 Haptic/Sound Service。开发模型不得自行决定额外触觉节点。

## 唯一触觉事件

HAPTIC\_TAP HAPTIC\_LOCK HAPTIC\_FAKE HAPTIC\_BURST

## HAPTIC\_TAP

只允许：Home 开始游戏；Bomb Core ACTION\_DOWN；合法 ACTION\_UP 进入 Handoff；Lose 查看挑战；Challenge 去完成。短、轻、单次。

## HAPTIC\_LOCK

只允许在按压首次达到 1100ms 合法阈值时触发一次。合法松手进入 Handoff 时禁止再次 Lock。

## HAPTIC\_FAKE

Fake 50–110ms 区段一次，明显弱于 Burst。

## HAPTIC\_BURST

Explosion E2 一次，全 App 最强。

## 唯一声音事件

SOUND\_PRESS\_AMBIENT SOUND\_FAKE SOUND\_EXPLOSION SOUND\_REVEAL

SOUND\_PRESS\_AMBIENT：低音量电流/脉冲氛围；只在 Bomb 页面；Waiting 极低；Pressing 正常；Handoff 降低；后台或离开 Bomb 必须停止。

SOUND\_FAKE：短促高频 glitch，与 Explosion 明显不同。

SOUND\_EXPLOSION：E2 同步；峰值一次；尾音不超过约 700ms。

SOUND\_REVEAL：Lose 是你。 出现时极轻提示，不抢 Explosion。

## 禁止

背景音乐、每个按钮都震、随机音效、儿童 BOOM、人声呻吟素材。

## 验收

A01 只有四类触觉；A02 Lock 一次；A03 合法 release 使用 Tap；A04 Burst 最强；A05 Bomb 离开后 Ambient 停止。
