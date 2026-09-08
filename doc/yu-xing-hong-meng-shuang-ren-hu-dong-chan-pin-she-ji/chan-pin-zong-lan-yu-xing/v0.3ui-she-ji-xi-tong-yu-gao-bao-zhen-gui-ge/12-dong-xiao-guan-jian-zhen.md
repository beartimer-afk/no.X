# 12｜动效关键帧

## 动效哲学

余兴的动效不是“页面切换动画”，而是建立物体连续性。用户应该感觉同一个 Core / Card 在不同阶段变形，而不是一页页跳转。

## 时间档位

* Micro：90–140ms
* Fast：160–220ms
* Standard：260–360ms
* Scene：450–650ms
* Emotional Pause：250–1200ms（不是动画，而是有意留白）

超过 700ms 的非游戏循环转场原则上禁止。

## Easing

* Press：快速 ease-out；
* 吸附 / 磁吸：spring，低回弹；
* 页面进入：decelerate；
* Explosion：前 200ms 极快，后段快速衰减；
* Secret Match Merge：缓慢靠近 + 轻吸附，不弹跳。

禁止大面积 Overshoot / Bouncy 动效，容易变幼稚。

## 关键转场

### Home → Round

START press → Core 收缩 → 背景变暗 → Core 放大占据舞台 → Round 标签 Fade In。

总 520ms 左右。

### Round Ready → Bomb

`3 2 1` 不使用传统大数字逐个弹跳。采用弱数字或 3 次 Core 脉冲，最后一次进入 Breathing。

### Explosion → Lose

按 06 章严格执行，中间保留真空停顿。

### Lose → Challenge Seed

“是你。”向上淡出 12vp；残余 Core 从 0.6 scale 拉成长方形种子；360–480ms 后形成 Hidden Card。

### Hidden → Reveal

用户拖动到阈值后卡片以 260–340ms 展开；正文在展开 55–70% 后才开始 Fade In，避免文字提前泄露。

### Challenge Complete → Next Move

卡片缩到 0.96；边缘光扫过；整体向后退；Next Move 从下方 16vp 进入。

### Next Round

Next Move 完成后不返回中间页，Core 从卡片后景重新聚合，形成连续循环。

## Reduce Motion

开启系统“减少动态效果”后：

* Core 呼吸幅度降低 60%；
* 禁用空间漂移 / 碎片；
* 页面转场用 Fade + 小幅 Scale；
* Explosion 保留触觉和亮度闪变，但不做大幅膨胀；
* 游戏时序不改变。

## 动效验收方式

每个关键转场必须录制 60fps 屏幕录像逐帧检查：

* 是否掉帧；
* 是否出现对象瞬移；
* 文案是否提前闪现；
* 触觉发生时视觉是否有对应事件；
* 动效结束后是否稳定停在像素对齐状态。
