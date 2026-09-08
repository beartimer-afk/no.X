# 20｜TASK-017：固定 UI 截图清单与像素验收

## 目标

统一模拟器/真机截图，禁止开发模型凭“差不多”判断 UI 完成。

## 基准设备

优先使用接近 390vp 内容宽的 HarmonyOS 手机模拟器/真机。若设备宽不同，必须同时提交尺寸信息。

## 必交 15 张截图

```
UI01_HOME.png
UI02_ROUND_READY.png
UI03_BOMB_WAITING.png
UI04_BOMB_PRESSING_EARLY.png
UI05_BOMB_PRESSING_VALID.png
UI06_BOMB_HANDOFF.png
UI07_BOMB_UNEASY.png
UI08_BOMB_TENSE.png
UI09_FAKE_SIGNAL_PEAK.png
UI10_EXPLOSION_FLASH.png
UI11_EXPLOSION_VACUUM.png
UI12_LOSE_REVEAL_LOCKED.png
UI13_LOSE_REVEAL_READY.png
UI14_CHALLENGE_REVEAL.png
UI15_CHALLENGE_COMPLETED.png
```

## 每张图人工/模型核对顺序

1. 页面是否为正确状态；
2. 是否多了未定义组件；
3. 左右边距；
4. 主焦点位置；
5. 字号层级；
6. 颜色；
7. 圆角；
8. 发光强弱；
9. 按钮数量；
10. 文案是否逐字一致。

## P0 UI 错误

出现以下任意一个直接 FAIL：

* 系统蓝主按钮；
* 卡通炸弹；
* 首页宫格；
* Core 尺寸明显错误；
* 文案与合同不同；
* Lose 页提前显示 Challenge；
* Explosion 无 Vacuum；
* 横向内容溢出；
* 白底闪烁；
* 主按钮数量错误。

## P1 UI 错误

* 边距误差 >4vp；
* 字号层级明显错；
* 发光过强导致文字不可读；
* Card 圆角错误；
* 按钮高不为 56vp；
* Core 未居中。

## P2

轻微抗锯齿、字体平台差异，可记录后接受。

## 禁止修复方式

发现一个页面偏移时，禁止全局乱改 Token。先确认是页面局部还是 Token 系统性错误。

## 验收结果格式

```
UI01: PASS
UI02: FAIL P1 - Core center +8vp Y
...
```

必须逐张输出。
