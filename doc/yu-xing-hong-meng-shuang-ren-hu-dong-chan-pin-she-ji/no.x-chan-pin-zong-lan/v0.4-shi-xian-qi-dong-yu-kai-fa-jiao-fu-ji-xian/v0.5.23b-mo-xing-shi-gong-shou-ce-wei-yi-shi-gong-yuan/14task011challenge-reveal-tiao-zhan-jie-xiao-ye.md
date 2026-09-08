# 14｜TASK-011：Challenge Reveal 挑战揭晓页

## 目标

实现 CHALLENGE\_REVEAL。挑战内容只能在用户点击 查看挑战 后显示。

## 依赖

TASK-010 PASS。

## 页面固定结构

顶部左关闭按钮：命中44×44vp，内部必须使用 `$media:nox_ic_close`，视觉24×24vp；顶部中 NO.X 15fp；顶部右挑战序号如 1 / 20；标题 你的挑战；中央 NoxChallengeCard；主按钮 去完成；次按钮 下一张；底部说明 更大胆的问题，更亲密的连接。。

## Challenge Card

左右24vp；宽=内容区100%；最小高280vp；Radius24vp；COLOR\_SURFACE；弱紫内渐变；Border1vp COLOR\_PURPLE opacity60%；padding24vp。

Card 内唯一图标固定为 `$media:nox_ic_heart`，视觉20×20vp。**禁止在 heart/flame 中自行选择。** 其后依次：标签 挑战 13fp；正文26fp最多6行；可选 hint15fp；不显示星级/积分。

## 测试 Challenge

currentChallengeId=challenge\_test\_01；文本：看着对方的眼睛，认真说一句你平时不太敢说的话。。本任务首屏禁止随机。进入页面时 challengeCompleted=false。

## 主按钮 去完成

点击：轻触觉；challengeCompleted=true；UI进入完成态。禁止相机、计时、验证。

完成态：Card不变；主按钮改为 下一轮；次按钮隐藏。点击下一轮只发送 NEXT\_ROUND\_REQUESTED，TASK-012 才实现导航。

## 次按钮 下一张

未完成态只在 challenge\_test\_01 与 challenge\_test\_02 间循环；currentChallengeId 同步更新；challengeCompleted保持false。

## 关闭

点击 nox\_ic\_close 后弹自定义确认：结束这次游戏？ / 当前回合会结束。 / 继续游戏 / 结束。禁止直接退出。

## 禁止

录音录像、分享、收藏、点赞评论、失败验收、页面局部 boolean 代替 challengeCompleted、下载第三方 icon、把 heart 换成 flame。

## 必交

TASK011\_challenge\_initial.png TASK011\_challenge\_completed.png TASK011\_challenge\_next.mp4 TASK011\_exit\_confirm.png

## 验收

A01 Lose 不泄露 Challenge；A02 Card 不是弹窗；A03 去完成只设置 challengeCompleted；A04 下一张同步 currentChallengeId；A05 完成态只剩下一轮；A06 退出有确认；A07 Close= nox\_ic\_close；A08 Card Icon=nox\_ic\_heart。
