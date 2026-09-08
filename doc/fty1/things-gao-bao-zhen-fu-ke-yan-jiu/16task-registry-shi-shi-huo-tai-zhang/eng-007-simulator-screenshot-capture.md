# ENG-007｜Simulator Screenshot Capture

**Status:** `SPEC_READY`\
**Dependencies:** ENG-005

## Goal

对固定 DevVisualLab route/fixture 产出 deterministic 模拟器截图，并建立 actual 目录合同。

## Required Docs

* H00｜Visual Acceptance Harness
* 07.3｜Golden Screenshot、Geometry 与 Diff
* 17｜Evidence Asset & Measurement Pipeline

## Output Contract

```
visual-test/actual/<component>/<fixture>.png
```

报告必须记录：device profile、OS/SDK、orientation、language、font scale、fixture ID、capture timestamp。

## Rules

* Canonical acceptance device 固定为 Phone / Portrait / Light / zh-CN / font scale 1.0
* 截图前 fixture 状态固定：scroll offset、keyboard、overlay、selection 均明确
* 不允许人工裁剪 actual 之后再称为原始截图；裁剪步骤必须由工具可复现

## Acceptance

* 同一 fixture 连续截图尺寸一致
* PNG 非空、可读
* 输出路径 deterministic
* Capture metadata JSON 同步生成
