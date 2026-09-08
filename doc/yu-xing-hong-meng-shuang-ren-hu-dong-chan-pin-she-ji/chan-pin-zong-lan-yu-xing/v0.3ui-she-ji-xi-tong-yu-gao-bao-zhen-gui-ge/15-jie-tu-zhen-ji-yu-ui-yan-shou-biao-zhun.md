# 15｜截图、真机与 UI 验收标准

## 验收不是“和设计稿差不多”

余兴的 UI 需要三层验收：静态截图、录屏动效、真机感官。

## A. 静态截图

必须覆盖：

1. Home Idle
2. Round Ready
3. Bomb Calm
4. Bomb Tense
5. Lose Reveal
6. Hidden Challenge
7. Revealed Challenge
8. Secret Match Handoff
9. Secret Match Choice
10. Match Result
11. Packs
12. Editor
13. Settings
14. Summary
15. Media Viewer

### 截图检查项

* 主视觉焦点是否唯一；
* 水平边距是否一致；
* 字号 / 字重是否遵循 Token；
* 是否出现未经定义的颜色；
* 圆角是否混乱；
* 是否有系统默认组件未品牌化；
* 是否存在“为了填空而填”的文字 / 图标；
* 25% 缩略图下层级是否仍清楚。

## B. 动效录屏

至少录：

1. Home → Bomb
2. Hold → Pass
3. Fake Signal
4. Explosion → Lose
5. Lose → Challenge Reveal
6. Challenge → Next Round
7. Secret Match A → Handoff → B

### 录屏检查

* 60fps 是否稳定；
* 对象是否有连续性；
* 动效是否过长；
* 是否出现廉价弹跳 / 粒子滥用；
* Fake 与 Explosion 是否足够不同；
* Emotional Pause 是否存在。

## C. 真机感官

必须真机测试：

* 暗室；
* 普通室内光；
* 系统音量低 / 高；
* 触觉开 / 关；
* Reduce Motion；
* 小屏 / 标准屏至少各一台或模拟器截图 + 真机主设备。

## 关键主观门槛

试玩后直接问：

1. 你觉得它像什么类型的 App？
2. 有没有哪一刻觉得“廉价 / 土 / 幼稚”？
3. Bomb 哪一刻开始紧张？
4. 爆炸之后，你有没有期待下一张挑战？
5. 你愿不愿意马上再玩一轮？

如果多数人第一反应是“真心话大冒险 App”或“喝酒小游戏”，说明品牌视觉没有拉开差距。

## 零容忍项

出现以下任一问题，不进入后续功能开发：

* 卡通炸弹；
* 粉紫大渐变作为主视觉；
* 首页宫格；
* 默认系统 Button 直接使用；
* 结果页彩纸庆祝；
* Challenge 像普通表单卡；
* Secret Match 能看到上一人的信息；
* Explosion 掉帧；
* 触觉和动画明显错位。

## v0.3 UI Definition of Done

只有满足以下条件才算 UI 基线成立：

* 15 张核心静态截图全部通过；
* 7 条核心动效录屏全部通过；
* Bomb 真机至少试玩 10 个 Round 无明显规律感；
* Secret Match 手机交接无信息泄露；
* 用户不用教程可以从 Home 开始并完成一整轮；
* 没有关键页面需要开发者自行决定视觉方案。
