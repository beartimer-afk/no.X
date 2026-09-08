# ENG-004｜Fixture Registry

**Status:** `SPEC_READY`\
**Dependencies:** ENG-002、ENG-003

## Goal

建立 deterministic Fixture Registry。任何视觉/交互测试都不得临时创建随机数据。

## Required Docs

* H00｜Visual Acceptance Harness
* 07.2｜DevVisualLab 与 Fixture
* 02.8｜Canonical Domain Schema

## Allowed Files

* `visualtest/fixtures/**`
* Fixture schema/registry 纯测试

## First Fixture IDs

```
foundation_empty
checkbox_unchecked
checkbox_checked
checkbox_disabled
todo_normal
todo_completed
todo_deadline_today
todo_repeat
todo_long_title
expanded_empty
expanded_notes
checklist_mixed_height
project_standard
area_standard
heading_standard
```

## Rules

* Fixture ID 永久稳定
* 日期使用固定 TestClock，不读取系统今天
* ID 固定，不使用随机 UUID
* 文案固定
* fixture 不访问真实 DB/网络

## Acceptance

* 同一 fixture 连续加载两次 deep-equal
* TestClock 改变前输出 deterministic
* Registry 不存在 ID 时返回明确错误
* Build/unit test PASS
