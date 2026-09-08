# 06｜页面清单与逐屏交互

## 1. 页面层级

第一版控制在以下核心页面：

1. Home
2. Secret Match Privacy Gate
3. Secret Match Choice
4. Hand-off Gate
5. Match Result
6. Round Ready
7. Bomb
8. Lose Reveal
9. Challenge Hidden
10. Challenge Revealed
11. Timed / Audio / Photo / Video Executor
12. Challenge Complete
13. Next Move
14. Pause Sheet
15. Session Summary
16. Packs
17. Pack Detail
18. Pack Editor
19. Challenge Editor
20. Settings

不单独做登录、社区、聊天、好友、个人主页。

## 2. Home

### 信息层级

视觉中心：**START**。

次级入口：Packs / Create / Settings。

可在 START 下方以极弱文本显示当前 Pack，例如“当前：默认挑战”。

### 操作

* 点 START：Quick Start；
* 长按或旁边小入口：展开“Secret Match”；
* 不弹人数选择；
* 不弹场景选择；
* 不弹模式问卷。

## 3. Round Ready

页面只承担重新聚焦注意力。

建议：

* 顶部小字 ROUND 03；
* 中央 BOMB；
* 2 秒内自动进入。

整个页面不可承载规则设置。

## 4. Bomb

屏幕只保留：

* 中央核心视觉；
* 极简 PASS 状态提示；
* 顶部很弱的暂停入口。

不显示计时、不显示概率、不显示比分。

## 5. Lose Reveal

爆炸结束后独占一屏：

> 是你。

默认 1 秒后出现“看看挑战”，或用户点击继续。

Challenge 不与输家结果同屏同时出现。

## 6. Challenge Hidden

卡片背面占主要视觉空间。

文案可为：

> 挑战已确定

交互：点击 / 上滑揭晓。必须是主动动作。

## 7. Challenge Revealed

正文最大。底部操作按类型变化。

普通：完成 / 换一个。 限时：开始 / 换一个。 媒体：开始录音/拍照/录像 / 换一个。

## 8. Next Move

只给 2 个清晰选项，避免决策负担。

例如：

* 下一轮更刺激一点
* 下一轮换个游戏

选择后立即进入下一轮，不再弹确认。

## 9. Pause Sheet

采用底部 Sheet 或轻覆盖层，不跳出 Session 视觉语境。

只提供：

* 继续；
* 本局设置；
* 结束游戏。

“本局设置”只允许修改不破坏当前状态的参数，例如声音、触觉、难度范围；不允许在 Bomb 运行中改随机逻辑。

## 10. Session Summary

展示：轮数、时长、完成数、跳过数、临时媒体数量。

主要操作：再来一局 / 结束。

如果有临时媒体，结束前必须出现媒体处理卡：全部删除 / 查看并处理。

## 11. 导航原则

* Session 内尽量无传统底部导航；
* 返回手势不能让用户意外穿过秘密交接边界；
* Home 与编辑管理区可使用标准导航；
* Session 属于沉浸模式，所有非核心入口降权。
