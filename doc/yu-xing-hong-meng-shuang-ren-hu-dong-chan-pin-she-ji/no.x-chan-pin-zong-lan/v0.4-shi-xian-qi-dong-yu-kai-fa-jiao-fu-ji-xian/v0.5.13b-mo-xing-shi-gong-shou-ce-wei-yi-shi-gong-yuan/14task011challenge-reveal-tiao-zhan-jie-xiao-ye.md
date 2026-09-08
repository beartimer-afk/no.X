# 14｜TASK-011：Challenge Reveal 挑战揭晓页

## 目标

实现 CHALLENGE\_REVEAL。挑战内容只能在用户点击 查看挑战 后显示。

## 依赖

TASK-010 PASS。

## 页面固定结构

顶部左关闭 ×（44vp）；顶部中 NO.X 15fp；顶部右挑战序号如 1 / 20；标题 你的挑战；中央 NoxChallengeCard；主按钮 去完成；次按钮 下一张；底部说明 更大胆的问题，更亲密的连接。。

## Challenge Card

左右 24vp；宽=内容区 100%；最小高 280vp；Radius 24vp；COLOR\_SURFACE；允许弱紫内渐变；Border 1vp COLOR\_PURPLE opacity 60%；padding 24vp。

Card 内：20vp 已冻结线性图标；标签 挑战 13fp；正文 26fp 最多 6 行；可选 hint 15fp；不显示星级/积分。

## 测试 Challenge

currentChallengeId = challenge\_test\_01；文本：看着对方的眼睛，认真说一句你平时不太敢说的话。。本任务首屏禁止随机。

进入页面时必须 challengeCompleted=false。

## 主按钮 去完成

点击：轻触觉；challengeCompleted=true；UI 进入完成态。禁止相机、计时、验证。

完成态：Card 不变；主按钮改为 下一轮；次按钮隐藏。

点击 下一轮：只发送 NEXT\_ROUND\_REQUESTED，TASK-012 才实现导航。

## 次按钮 下一张

未完成态可在两条测试挑战间循环：challenge\_test\_01 与 challenge\_test\_02。切换时 currentChallengeId 必须同步更新，challengeCompleted 仍为 false。

## 关闭 ×

弹自定义确认：结束这次游戏？ / 当前回合会结束。 / 继续游戏 / 结束。禁止直接退出。

## 禁止

录音录像、分享、收藏、点赞评论、失败验收、页面局部 boolean 代替 challengeCompleted。

## 必交

TASK011\_challenge\_initial.png TASK011\_challenge\_completed.png TASK011\_challenge\_next.mp4 TASK011\_exit\_confirm.png

## 验收

A01 Lose 不泄露 Challenge；A02 Card 不是弹窗；A03 去完成只设置 challengeCompleted；A04 下一张同步 currentChallengeId；A05 完成态只剩下一轮；A06 退出有确认。
