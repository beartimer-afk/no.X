# 10｜TASK-007：Bomb 紧张视觉阶段 CALM/UNEASY/TENSE

## 目标

根据累计按压预算消耗比例提升 Bomb 视觉紧张度。只改变 visualPhase，不得新增/切换业务 GameState。

## 依赖

TASK-006 PASS。

## 紧张度唯一计算

若当前不是 Pressing：effectiveHoldMs = bombAccumulatedHoldMs。

若当前为 Pressing：effectiveHoldMs = bombAccumulatedHoldMs + (now - pressStartAt)。

然后：ratio = clamp(effectiveHoldMs / bombTargetHoldMs, 0, 1)。

禁止使用 wall-clock now - roundStart。

## 视觉阶段

CALM：ratio < 0.45。visualPhase=CALM；Core 基础粉紫光；呼吸周期 1800ms；无电裂纹；标题 按住并回答。

UNEASY：0.45 ≤ ratio < 0.72。visualPhase=UNEASY；外环亮度 +15%；呼吸周期 1200ms；背景极弱紫雾；Prompt Card 不变；标题仍为 按住并回答。

TENSE：ratio ≥ 0.72。visualPhase=TENSE；外环粉色更亮；COLOR\_DANGER 仅少量高光；呼吸周期 720ms；外环允许 1–2px 视觉抖动；标题改为 继续按住…；次文案 越来越热了。；Prompt 不变。

## 业务状态规则

CALM/UNEASY/TENSE 期间，GameState 仍只能是 BOMB\_WAITING\_PRESS、BOMB\_PRESSING、BOMB\_HANDOFF。禁止写 state = BOMB\_TENSE。

## 禁止

百分比、可计算进度环、剩余秒、Tense 改 1100ms、Tense 改 target、Waiting/Handoff 的真实时间推进 ratio。

## 必交

TASK007\_calm.png TASK007\_uneasy.png TASK007\_tense.png TASK007\_tension\_transition.mp4

## 验收

A01 三阶段可区分；A02 无可计算进度；A03 ratio 只由累计按压产生；A04 visualPhase 不改变 GameState；A05 Prompt 不变。
