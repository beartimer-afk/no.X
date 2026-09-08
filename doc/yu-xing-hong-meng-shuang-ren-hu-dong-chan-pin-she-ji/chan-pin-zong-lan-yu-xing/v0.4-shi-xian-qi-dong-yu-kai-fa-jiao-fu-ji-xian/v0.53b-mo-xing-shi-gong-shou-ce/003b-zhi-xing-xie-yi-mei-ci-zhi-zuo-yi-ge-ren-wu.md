# 00｜3B 执行协议：每次只做一个任务

## 强制执行方式

**禁止一次性要求模型实现整个 App。** 每次调用开发模型时，只允许给它一个 `TASK-XXX` 原子任务包。

## 每个任务固定 8 步

1. 复述任务编号与目标；
2. 列出将修改的文件；
3. 检查依赖任务是否已通过；
4. 只实现本任务；
5. 构建并运行；
6. 生成规定截图/录屏；
7. 逐条回答验收 PASS/FAIL；
8. STOP，等待人工验收。

## 固定回复格式

```
TASK: TASK-XXX
STATUS: READY | BLOCKED | IMPLEMENTED | FAILED

FILES TO CHANGE:
- ...

FILES CHANGED:
- ...

SPEC_GAP:
- NONE
或
- <缺失项>

BUILD:
- PASS / FAIL

ACCEPTANCE:
- A01 PASS
- A02 PASS
- A03 FAIL: <原因>

ARTIFACTS:
- screenshot_xxx.png
- recording_xxx.mp4

NEXT ACTION:
- STOP，等待验收
```

## 禁止行为

* 禁止回复“我会继续完成剩余部分”；
* 禁止未实际构建却声称构建通过；
* 禁止修改任务之外页面；
* 禁止擅自换颜色、字体、字号、圆角；
* 禁止自动加动画/渐变/功能；
* 禁止把精确值改成“约”“大约”“自适应即可”。

## SPEC\_GAP

没有定义的具体决策：不猜、不用默认值、标记 `SPEC_GAP`。如果不影响其他部分可继续其余项，但不得隐藏缺口。
