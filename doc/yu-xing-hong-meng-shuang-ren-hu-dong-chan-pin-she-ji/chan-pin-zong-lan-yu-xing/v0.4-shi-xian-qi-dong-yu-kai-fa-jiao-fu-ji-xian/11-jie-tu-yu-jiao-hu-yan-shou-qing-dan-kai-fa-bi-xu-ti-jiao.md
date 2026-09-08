# 11｜截图与交互验收清单（开发必须提交）

每个里程碑必须提供模拟器 / 真机截图与录屏。没有视觉证据，不视为完成。

## 固定截图 12 张

文件名必须一致：

1. `01_home.png`
2. `02_ready.png`
3. `03_bomb_idle.png`
4. `04_bomb_press_200ms.png`
5. `05_bomb_press_1100ms.png`
6. `06_bomb_warm.png`
7. `07_bomb_tense.png`
8. `08_handoff.png`
9. `09_explosion_peak.png`
10. `10_lose.png`
11. `11_challenge.png`
12. `12_end_sheet.png`

## 固定录屏 5 条

1. `flow_full.mp4`：Home 到下一 Round 完整闭环；
2. `hold_early_release.mp4`：不足 1100ms 松手；
3. `hold_pass.mp4`：合法松手与 Handoff；
4. `fake_signals.mp4`：两类 Fake；
5. `explosion.mp4`：Explosion → 真空 → 是你。

## 逐项验收

### Home

* Logo 是资产而非文本近似；
* CTA 高度 56vp；
* 无 Tab Bar；
* 主题只有暗紫 / 粉 / 紫。

### Ready

* Core 138vp；
* 不显示 HOLD；
* 文案逐字一致。

### Bomb

* Prompt Card 不随 Core 抖；
* Core 按下反馈 <100ms 可感知；
* 1100ms 前松手不 Pass；
* 1100ms 后松手进入 Handoff；
* Handoff 期间 Bomb 不累计；
* 不显示真实倒计时。

### Explosion

* 收缩 → 释放 → 碎散 → 真空 → Lose 顺序完全一致；
* 真空时屏幕没有文案；
* 无系统 Snackbar / Toast；
* 不掉帧到肉眼明显。

### Challenge

* 卡片位置固定；
* 正文不小于 18sp；
* `去完成` 与 `下一张` 同时存在；
* 下一张不弹确认。

## P0 视觉 Bug

出现以下任一问题，版本不可进入下一里程碑：

* 使用系统默认蓝色；
* Core 被改成实体卡通炸弹；
* Explosion 明显卡顿；
* 主 CTA 尺寸 / 颜色明显偏离；
* 文案被 AI 自行改写；
* Home 出现额外功能入口；
* Bomb 显示剩余时间；
* Handoff 没有暂停风险时间；
* 按压手感无法稳定复现。
