# 20｜3B Handoff｜开工与自动调度说明

## 第一个任务

从 `ENG-001｜Create HarmonyOS 26 Application Skeleton` 开始。3B 第一次进入只读 Developer Handbook + 当前 Capsule 的 Required Docs；不得问用户“还要读哪些文档”。

## 调度

调度器读取 Task Registry，选择 Dependencies 满足、status=READY、无用户输入依赖的最小 Capsule。3B 完成后输出 Machine Report；调度器根据 PASS/FAIL/BLOCKED 更新 Registry，并生成下一 Capsule 或 Fix Capsule。

## 失败处理

Build fail → 先修 build；Structure/Geometry/Typography/Color/Motion 按优先级逐层处理。不得通过修改 Golden、放宽 threshold、改 LOCKED Token 来让测试绿。

## Evidence blocker

需要 iPhone 真机证据时标 `BLOCKED:PENDING_EVIDENCE`，记录需要哪一段录屏/截图/动作；立即转执行其他 READY Capsule。

## 自动验收链

`Spec → Fixture → Implement → DevVisualLab → Simulator Capture → Inspector Geometry → Diff → Machine Report → Fix → VERIFIED → LOCKED`。

## 交付物

每个 Capsule：代码 diff、测试、截图、geometry JSON、diff report、machine report。每个 Milestone：registry snapshot、Golden index、未决 Pending、回归结果。

## 开工结论

纯规格层已经达到可以开始真实 HarmonyOS 工程实施的状态。下一步不应继续把“写文档”当作实施本身；必须创建真实工程并开始 ENG-001。
