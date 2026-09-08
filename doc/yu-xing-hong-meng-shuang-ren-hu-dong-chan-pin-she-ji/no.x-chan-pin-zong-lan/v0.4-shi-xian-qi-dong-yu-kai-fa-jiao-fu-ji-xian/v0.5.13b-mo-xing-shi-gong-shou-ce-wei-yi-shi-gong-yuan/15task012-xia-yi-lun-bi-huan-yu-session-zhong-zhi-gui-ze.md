# 15｜TASK-012：下一轮闭环与 Session 重置规则

## 目标

让 Challenge Reveal → 下一轮 → Round Ready → Bomb 连续循环至少 5 轮且无上一轮残留。

## 依赖

TASK-011 PASS。

## 点击 下一轮 的唯一重置顺序

1. roundIndex += 1；
2. state = ROUND\_TRANSITION；
3. pressStartAt=null；pressSafeUntil=null；isPressValid=false；
4. bombTargetHoldMs=0；bombAccumulatedHoldMs=0；
5. fakeSignalCount=0；fake1ThresholdMs=null；fake2ThresholdMs=null；fake1Triggered=false；fake2Triggered=false；
6. visualPhase=CALM；
7. currentChallengeId=null；challengeCompleted=false；
8. 等待 280ms；
9. 进入 ROUND\_READY；
10. Round Label 使用两位数字 ROUND 02、ROUND 03…；
11. 只有再次点击 我准备好了，才为新轮生成 target 与 Fake thresholds。

## holderIndex

新一轮由上一轮输家以外的另一人先拿：holderIndex = loserIndex == 0 ? 1 : 0。完成赋值后 loserIndex=null。

若 loserIndex 异常为空，holderIndex=0 并记录日志。

## 不得残留

Tense、Fake 阈值/触发标志、上一轮 Prompt、Challenge 完成态、Explosion 粒子、按钮锁定计时、上一轮 target/accumulated。

## 必交

连续 5 轮录屏；每轮日志列 roundIndex / target / 初始 holder / fake thresholds。

## 验收

A01 roundIndex 正确；A02 每轮重新点 Ready；A03 每轮 target 只生成一次；A04 无视觉/数据残留；A05 holder 规则正确；A06 连续 5 轮无崩溃。
