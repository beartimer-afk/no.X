# 03｜Challenge 系统与执行规则

## 1. Challenge 的产品角色

小游戏只负责“谁来”。Challenge 决定“接下来发生什么”。

因此每一轮有两次揭晓：

1. **谁输了**；
2. **输家要做什么**。

两者必须分开，不在爆炸结束时一次性显示。

## 2. Challenge 最小数据模型

* id；
* packId；
* title / body；
* type：standard / timed / audio / photo / video；
* difficulty：1–5；
* tags\[]；
* weight：默认 1.0；
* durationSec：可空；
* verification：self / partner / media；
* repeatPolicy；
* enabled；
* createdBy：official / user；
* safetyFlags（只用于本地功能约束，不解释用户文本语义）。

## 3. 挑战抽取

从当前 Session Challenge Pool 中抽取。

默认规则：

* 已完成挑战在本 Session 内降低权重 90%；
* 最近 3 轮出现过的挑战不得立即重复；
* 被 Skip 的挑战本 Session 内默认不再出现；
* 如果剩余可用挑战 <3，允许恢复旧挑战，但优先选择最久未出现的；
* Secret Match 只改变“可进入池的挑战”，不改变随机算法。

## 4. Reveal

### Hidden Card

先显示卡背，不显示正文。

用户主动点击/上滑揭晓。

### Revealed Card

主视觉只保留：

* 挑战正文；
* 必要时显示时长；
* 媒体类型图标；
* 难度 1–5。

不显示权重、内部标签、Pack 技术信息。

## 5. Challenge Type

### Standard

现场完成。主按钮：**完成**。次按钮：**换一个**。

### Timed

先显示挑战，再点击开始。进入全屏倒计时。倒计时到 0 不自动判定失败，只提示“时间到”，由双方决定完成/换一个。

### Audio

进入 App 内录音。结束后：试听 / 重录 / 使用。

### Photo

进入 App 内相机。拍摄后：使用 / 重拍。

### Video

进入 App 内录像。录像应有明确最长时长，第一版默认 30 秒，可在挑战中缩短。

## 6. Skip / 换一个

“任何挑战都可以换一个”是硬规则。

文案使用：

* 换一个
* 跳过

避免：

* 认输
* 胆小鬼
* Chicken
* 惩罚加倍（除非用户主动开启这种自定义规则且表述不强迫）

连续换 3 个后，底部出现：

> 这轮不太对胃口？降低强度

点击后仅本轮从较低 difficulty 抽取。

## 7. 验收

默认 Standard Challenge 不需要复杂验收。

推荐：完成后由持机者点“完成”。

Partner Verification 只在挑战显式要求时出现两个按钮：

* 完成
* 再来一次

“再来一次”不应增加羞辱或惩罚，仅重新执行本挑战。

## 8. Challenge 完成反馈

反馈必须短：0.5–1 秒。

形式可以是：

* 卡片收拢；
* 轻触觉；
* 简短文字“完成”。

不要做积分爆炸或过度奖励，避免破坏整体高级感。

## 9. Next Move

Next Move 不是每轮强制。

默认出现频率：约每 2–3 轮一次，或由 Pack 自定义。

候选类型：

* 选择下一轮游戏；
* 下一轮挑战难度 +1 / -1；
* 从两个挑战方向中选一个；
* 临时写一个挑战加入本 Session；
* 禁用某一类挑战一轮。

第一版建议只做前两种，避免编辑流程打断现场节奏。
