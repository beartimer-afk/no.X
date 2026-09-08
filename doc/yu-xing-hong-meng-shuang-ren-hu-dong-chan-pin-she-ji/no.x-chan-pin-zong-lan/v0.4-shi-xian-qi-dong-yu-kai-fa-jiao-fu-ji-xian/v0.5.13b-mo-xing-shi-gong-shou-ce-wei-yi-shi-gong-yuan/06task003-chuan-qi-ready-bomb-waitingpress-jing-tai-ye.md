# 06｜TASK-003：串起 Ready → Bomb WAITING\_PRESS 静态页

## 目标

完成第一个 UI 里程碑：Home → Round Ready → Bomb WAITING\_PRESS。Index 根据 state 切换页面。仍不实现长按、随机、Explosion、Handoff、Fake。

## 依赖

TASK-002 PASS。

## 允许修改

pages/Index.ets pages/RoundReadyPage.ets pages/BombPage.ets components/NoxBombCore.ets components/NoxPromptCard.ets constants/CopyConstants.ets model/GameState.ets model/GameSession.ets

## Ready → Bomb

点击 我准备好了：发送 READY\_CONFIRM\_TAP；currentPromptId=prompt\_test\_01；state=BOMB\_WAITING\_PRESS；Index 做 180ms cross-fade。本任务不生成 bombTargetHoldMs。

## Bomb 390×844

TopBar Y52–96：左返回 X16/44×44；中央 NO.X 15fp；右 ROUND 01 13fp；无 Settings。

Title：按住并回答；top118；32fp；居中。

Prompt Card：X42 Y174 W306 H96 Radius18；COLOR\_SURFACE opacity0.94；Border 1vp COLOR\_BORDER opacity0.90；padding20；17fp；prompt\_test\_01=说出你最想被亲吻的地方。；无图标。

Core：中心(195,424)；视觉外径156vp；预留命中188vp；静态。

### Bomb Core WAITING 每层必须一致

* Center Dot：10×10vp，COLOR\_TEXT\_MAIN opacity0.96；
* Inner Disk：92×92vp，COLOR\_SURFACE opacity0.72；
* Primary Ring：132×132vp，stroke3vp，COLOR\_PURPLE opacity0.88；
* Pressure Ring：156×156vp，stroke4vp，COLOR\_PINK opacity0.86；
* Glow 最大直径206vp：COLOR\_PURPLE opacity0.24 + COLOR\_PINK opacity0.22；
* 188vp 命中圆与视觉圆同中心，但本任务不绑定事件；
* 禁止心形、传统炸弹、HOLD、百分比、倒计时、旋转。

BottomHint baseline约674：按住它，说出来，别松手。；15fp COLOR\_TEXT\_SUB；居中。

## Back

尚未真正开始，点击 state=ROUND\_READY，不弹确认。

## 禁止

系统 Router、长按、Explosion、Handoff、随机、Fake、倒计时、百分比、Player。

## 必交

TASK003\_bomb\_waiting.png；TASK003\_home\_ready\_bomb.mp4（冷启动→开始游戏→我准备好了→Bomb）。

## 验收

A01 三屏可连续到达；A02 Index 唯一页面选择器；A03 Core 五层结构和中心/尺寸准确；A04 Card(42,174,306,96)；A05 无计时随机长按；A06 Back 回 Ready；A07 currentPromptId=prompt\_test\_01；A08 视觉一致。
