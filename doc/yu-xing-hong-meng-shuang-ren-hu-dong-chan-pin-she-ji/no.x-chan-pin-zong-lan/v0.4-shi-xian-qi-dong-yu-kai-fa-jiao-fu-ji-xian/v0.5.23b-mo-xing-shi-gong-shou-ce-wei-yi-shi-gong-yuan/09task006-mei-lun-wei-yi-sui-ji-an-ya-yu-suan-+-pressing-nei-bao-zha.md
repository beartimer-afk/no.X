# 09｜TASK-006：每轮唯一随机按压预算 + Pressing 内爆炸

## 目标

加入真正 Bomb 计时模型。Bomb **只累计手指实际按住 Core 的时间**，Explosion **只允许在 BOMB\_PRESSING 中发生**。

本任务先使用测试 EXPLOSION 黑屏标记，不做正式 Explosion 动画。

## 依赖

TASK-005 PASS。

## 允许修改

pages/RoundReadyPage.ets pages/BombPage.ets model/GameSession.ets constants/GameConstants.ets services/RandomService.ets utils/TimeUtils.ets

## 固定常量

BOMB\_MIN\_DURATION\_MS = 14000 BOMB\_MAX\_DURATION\_MS = 30000 NEW\_HOLDER\_SAFE\_MS = 400

## Round 初始化

每轮只在点击 我准备好了、准备进入 Bomb 时执行一次：

bombTargetHoldMs = randomIntInclusive(14000, 30000) bombAccumulatedHoldMs = 0 loserIndex = null fakeSignalCount = 0

不得在按下、松手、Handoff、Prompt 切换、Fake、前后台恢复时重抽 bombTargetHoldMs。

## 按压累计规则

BOMB\_WAITING\_PRESS 与 BOMB\_HANDOFF 不累计任何 Bomb 时间。

ACTION\_DOWN： pressStartAt = monotonicNowMs() pressSafeUntil = pressStartAt + 400

Pressing 中： currentHeldMs = monotonicNowMs() - pressStartAt effectiveHoldMs = bombAccumulatedHoldMs + currentHeldMs

真实爆炸条件： effectiveHoldMs >= bombTargetHoldMs 且 monotonicNowMs() >= pressSafeUntil。

满足后：

1. loserIndex = holderIndex；
2. state = EXPLOSION；
3. 停止触控；
4. 显示测试文字 BOOM\_TEST；
5. 本任务不进入 Lose。

## ACTION\_UP / CANCEL 必须先结算已按时间

若尚未 Explosion，离开 Pressing 前必须先执行：

heldMs = upOrCancelAt - pressStartAt bombAccumulatedHoldMs += max(heldMs, 0)

然后才执行短按失败或 Handoff。

即使短按 <1100ms 或滑出 CANCEL，这段已经真实按住的时间也计入 Bomb 预算，防止反复短按冻结 Bomb。

## 400ms 安全窗

如果 effectiveHoldMs 已达到 target，但当前按压尚不足 400ms：暂不爆；若仍保持按压，到 pressSafeUntil 立即爆。若提前松手/CANCEL，则按对应规则结算时间，holder 不因短按/取消而切换。

## 禁止

Waiting 爆炸；Handoff 爆炸；后台爆炸；使用 wall-clock bombExplodeAt；每次 PASS 重抽 target；UI 显示 14–30 秒、剩余值或百分比。

## 必交

20 轮日志，每轮列 bombTargetHoldMs；一轮详细日志列每次 heldMs 与 bombAccumulatedHoldMs；TASK006\_random\_explosion.mp4，必须证明 Explosion 发生时手指仍在按 Core。

## 验收

A01 target 全在 14000–30000；A02 一轮只生成一次；A03 Waiting/Handoff 不累计；A04 短按时间会累计；A05 400ms 安全窗有效；A06 Explosion 只发生在 Pressing；A07 UI 不泄露预算。
