# 00｜3B 执行协议：每次只做一个任务

## 唯一施工源

3B 施工时只允许使用本 **v0.5.2** 手册。禁止同时提供 v0.4、v0.3、v0.2。

### TASK-000A / TASK-000B 的上下文

只提供：

1. `00｜3B 执行协议`
2. `01｜工程目录、文件命名与禁止改动范围`
3. `02｜全局 Token`
4. `03A｜静态资源、App Icon、字体与 HarmonyOS 资源合同`
5. 当前一个 TASK

### TASK-001 及以后

只提供：

1. `00｜3B 执行协议`
2. `01｜工程目录、文件命名与禁止改动范围`
3. `02｜全局 Token`
4. `03｜状态机与数据模型`
5. `03A｜静态资源、App Icon、字体与 HarmonyOS 资源合同`
6. 当前一个 TASK

禁止把整本 GitBook 塞给模型。

## 每个任务固定 8 步

1. 复述任务编号与目标；
2. 列出允许修改的文件；
3. 检查依赖任务是否已 PASS；
4. 只实现本任务；
5. 实际构建并运行；
6. 生成规定截图/录屏/日志；
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
BUILD:
- PASS / FAIL
ACCEPTANCE:
- A01 PASS
ARTIFACTS:
- ...
NEXT ACTION:
- STOP，等待验收
```

## 禁止行为

* 禁止未实际构建却声称 PASS；
* 禁止改当前 TASK 之外的页面/文件；
* 禁止擅自换颜色、字体、字号、圆角；
* 禁止下载图片、字体、图标、声音；
* 禁止使用默认 App Icon 继续后续开发；
* 禁止新增第三方 icon 库、字体库、UI 库；
* 禁止把精确值改成“约”“大约”。

## 缺失项

需求未定义：`SPEC_GAP`。 设计侧资源包缺失：`BLOCK_ASSET_MISSING`。 不得用临时占位素材绕过。
