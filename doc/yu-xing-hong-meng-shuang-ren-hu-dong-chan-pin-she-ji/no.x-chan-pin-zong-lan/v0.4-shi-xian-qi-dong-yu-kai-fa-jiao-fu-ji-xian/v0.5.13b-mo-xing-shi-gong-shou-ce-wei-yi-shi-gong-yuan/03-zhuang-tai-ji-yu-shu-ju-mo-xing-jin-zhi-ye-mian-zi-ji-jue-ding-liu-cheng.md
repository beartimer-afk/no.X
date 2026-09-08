# 03｜状态机与数据模型：禁止页面自己决定流程

## 唯一合法 GameState

HOME ROUND\_READY BOMB\_WAITING\_PRESS BOMB\_PRESSING BOMB\_HANDOFF EXPLOSION LOSE\_REVEAL CHALLENGE\_REVEAL ROUND\_TRANSITION

TENSE 不是 GameState，只是 Bomb 视觉阶段。禁止新增 BOMB\_TENSE 主状态。

## BombVisualPhase

CALM UNEASY TENSE

visualPhase 只能影响颜色、光效、呼吸频率、标题文案，不得改变业务流程。

## 唯一主流程

HOME → ROUND\_READY → BOMB\_WAITING\_PRESS → BOMB\_PRESSING。

BOMB\_PRESSING 短按/取消 → BOMB\_WAITING\_PRESS。

BOMB\_PRESSING 合法松手且未爆 → BOMB\_HANDOFF → 1300ms 后 BOMB\_WAITING\_PRESS。

BOMB\_PRESSING 累计按压预算耗尽 → EXPLOSION → LOSE\_REVEAL → CHALLENGE\_REVEAL → ROUND\_TRANSITION → ROUND\_READY。

Explosion 只允许从 BOMB\_PRESSING 触发。Waiting、Handoff、后台不得 Explosion。

## GameSession 必须字段

roundIndex: number holderIndex: 0 | 1 loserIndex: 0 | 1 | null bombTargetHoldMs: number bombAccumulatedHoldMs: number pressStartAt: number | null pressSafeUntil: number | null isPressValid: boolean fakeSignalCount: number fake1ThresholdMs: number | null fake2ThresholdMs: number | null fake1Triggered: boolean fake2Triggered: boolean currentPromptId: string currentChallengeId: string | null challengeCompleted: boolean state: GameState visualPhase: BombVisualPhase

禁止使用 bombStartAt / bombExplodeAt 作为正式业务字段。禁止把 Fake 阈值、Challenge 完成态只存在页面局部变量。

## 冷启动默认值

roundIndex=1 holderIndex=0 loserIndex=null bombTargetHoldMs=0 bombAccumulatedHoldMs=0 pressStartAt=null pressSafeUntil=null isPressValid=false fakeSignalCount=0 fake1ThresholdMs=null fake2ThresholdMs=null fake1Triggered=false fake2Triggered=false currentChallengeId=null challengeCompleted=false state=HOME visualPhase=CALM

currentPromptId 在进入首个 Bomb 时写入第一条测试 Prompt。

## Bomb 时间模型

每轮只生成一次 bombTargetHoldMs = uniform integer \[14000,30000]；bombAccumulatedHoldMs = 0。

只有 BOMB\_PRESSING 中手指真实按住 Core 的时间累计。Waiting、Handoff、页面过渡、后台都暂停。

短按或最终 CANCEL 的这段已经真实按住的时间仍计入累计预算，防止反复短按冻结 Bomb。

Pressing 中：currentHeldMs = monotonicNowMs() - pressStartAt；effectiveHoldMs = bombAccumulatedHoldMs + currentHeldMs。

真实爆炸条件：effectiveHoldMs >= bombTargetHoldMs 且 monotonicNowMs() >= pressSafeUntil。

满足时当前 holderIndex 写入 loserIndex，进入 EXPLOSION。

## 新按压安全窗

每次 CORE\_DOWN：pressSafeUntil = pressStartAt + 400。刚接手前 400ms 不瞬爆。如果预算已耗尽且仍保持按压，则安全窗结束时爆炸。

## 固定常量

BOMB\_MIN\_DURATION\_MS = 14000 BOMB\_MAX\_DURATION\_MS = 30000 MIN\_VALID\_PRESS\_MS = 1100 HANDOFF\_PAUSE\_MS = 1300 NEW\_HOLDER\_SAFE\_MS = 400 MAX\_FAKE\_SIGNAL = 2

## 验收

Waiting/Handoff 不累计；只有 Pressing 累计；Explosion 永远发生在 Pressing；一轮只生成一次 bombTargetHoldMs；Fake 与 Challenge 状态均由 GameSession 持有。
