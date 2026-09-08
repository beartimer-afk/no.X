# 28｜任务依赖图与严格施工顺序

## 禁止并行乱做

v0.1 单线程施工。资源、UI、逻辑依次推进。

```mermaid
flowchart TD
    GATE[Precheck Gate] --> T000A[000A Resources + Font Freeze]
    T000A --> T000B[000B Layered App Icon]
    T000B --> T001[001 Home Static]
    T001 --> T002[002 Round Ready]
    T002 --> T003[003 Bomb Waiting]
    T003 --> T004[004 Press]
    T004 --> T005[005 Handoff]
    T005 --> T006[006 Hold Budget Explosion]
    T006 --> T007[007 Tension]
    T007 --> T008[008 Fake Signal]
    T008 --> T009[009 Explosion Animation]
    T009 --> T010[010 Lose Reveal]
    T010 --> T011[011 Challenge Reveal]
    T011 --> T012[012 Round Loop]
    T012 --> T013[013 Local Data]
    T013 --> T014[014 Haptic & Sound]
    T014 --> T015[015 Lifecycle]
    T015 --> T016[016 E2E]
    T016 --> T017[017 UI QA]
```

## 进入下一任务的固定 Gate

```
BUILD PASS
+ 当前 TASK 所有 Axx PASS
+ 必交截图/录屏/日志齐全
+ SPEC_GAP = NONE 或人工明确延期
+ 资源哈希（若本 TASK 涉及资源）PASS
+ Git commit 完成
```

任何一项缺失不得进入下一任务。

## Commit 固定前两项

* `NOX TASK-000A resources and font freeze`
* `NOX TASK-000B layered app icon`

之后继续一 TASK 一 commit。
