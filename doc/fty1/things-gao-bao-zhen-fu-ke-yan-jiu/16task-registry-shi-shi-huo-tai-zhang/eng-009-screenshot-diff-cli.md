# ENG-009｜Screenshot Diff CLI

**Status:** `SPEC_READY`\
**Dependencies:** ENG-007

## Goal

建立与 HarmonyOS 无业务耦合的图像对比工具，输出 actual/golden/diff/overlay 与结构化指标。

## Required Docs

* 07.3｜Golden Screenshot、Geometry 与 Diff
* 17｜Evidence Asset & Measurement Pipeline

## Allowed Files

* `tools/diff/**`
* diff 单测/fixtures

## Output Contract

```
visual-test/diff/<scope>/<fixture>.png
visual-test/overlay/<scope>/<fixture>.png
visual-test/reports/<scope>/<fixture>.json
```

Report 至少：image dimensions、pixel mismatch ratio、mean error、max error、mask applied、threshold profile、PASS/FAIL。

## Rules

* 字体区域允许独立 mask/tolerance profile
* 非文本几何区使用更严格阈值
* 不自动更新 Golden
* 尺寸不一致直接失败，不偷偷 resize actual

## Acceptance

* identical → PASS
* one-pixel synthetic delta → 可检测
* size mismatch → FAIL\_SIZE
* mask 区变化按 profile 处理
* 输出 deterministic JSON
