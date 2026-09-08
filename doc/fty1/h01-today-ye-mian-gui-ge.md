# H01 Today 页面规格

状态：SPEC\_READY

## 定位

Today 是用户每日行动入口，不负责项目管理，而负责展示当前需要关注的任务。

## 数据来源

Todo -> ViewModel -> Today Page -> Component Render

## 核心组件

* Header
* SectionHeader
* TodoRow
* Checkbox

## 交互

完成任务必须通过 Command 流程：

UI -> CompleteTodoCommand -> Repository -> State -> UI

## 验收

* 页面可显示任务
* 完成状态可变化
* 无直接 UI 修改数据
