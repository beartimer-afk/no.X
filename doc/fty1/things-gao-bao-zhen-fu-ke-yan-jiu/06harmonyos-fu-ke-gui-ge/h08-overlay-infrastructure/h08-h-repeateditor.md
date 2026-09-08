# H08-H｜RepeatEditor

Repeat 必须实现为 `Template/Series + generated Copy/Instance`，禁止给 Todo 加一个简单 `repeat=true` 或只存 RRULE 字符串后让页面自行解释。

## Repeat modes

1. **Regular**：固定日历节奏，与完成时间无关，允许前一份未完成时下一份仍生成。
2. **After Completion**：下一次 occurrence 由实际 `completedAt + interval` 推导。

规则计算属于 Repeat Engine / Coordinator，不属于 Editor。

## Identity

Series、Template、Copy 各自稳定 identity。`Create Next Copy` 是预物化下一 occurrence，必须按 series + occurrence identity 幂等；不是 Duplicate。

## Lifecycle

Pause 保留 series/template/history；Resume 延续同一 series；Stop 停止 repetition/将模板退出重复生命周期，不能实现为删除历史。当前 Pause/Resume/Stop 的精确菜单/确认仍 `PENDING_EVIDENCE`。

## One-time exception

用户重排某个 Regular Copy 时，必须区分“只改当前 copy”与“更新未来规则”；确切 iPhone 文案/按钮已存在研究证据但视觉仍需 Golden。After Completion current copy 改期不应把未来 recurrence 固定成同一日历日期。

## UI composition

Editor 负责编辑 frequency/interval/mode 与已证实规则字段；Calendar/date math 由 Repeat Engine 提供。禁止 3B 自行扩展未经证据确认的高级选项。

## Commands

`CreateSeries / UpdateSeriesRule / PauseSeries / ResumeSeries / StopSeries / CreateNextCopy / RescheduleOccurrence`。UI 只发 intent/result。

## Fixtures

regular daily、regular weekly、after-completion N days、existing copy、one-time reschedule、future-rule update、paused、stopped、create-next idempotency、batch 不支持态。

## Acceptance

Regular/After Completion 算法、identity、idempotency、pause/resume/stop state、current-vs-future scope、projection generation、Logbook history regression PASS。精确 editor presentation、lifecycle menu、feedback motion 保持 `PENDING_EVIDENCE`。
