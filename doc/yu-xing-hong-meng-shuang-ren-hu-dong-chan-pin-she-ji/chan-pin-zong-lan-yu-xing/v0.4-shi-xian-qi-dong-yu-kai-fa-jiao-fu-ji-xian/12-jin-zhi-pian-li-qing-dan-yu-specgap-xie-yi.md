# 12｜禁止偏离清单与 SPEC\_GAP 协议

## 禁止开发 AI 自行做的事情

* 自己重新命名 NO.X；
* 自己选择另一套主题色；
* 自己增加蓝 / 青霓虹；
* 自己把成人摄影换成插画；
* 自己加手指示意图；
* 自己把圆形 Core 换成 Bomb 图标；
* 自己增加“玩家 1 / 玩家 2”注册步骤；
* 自己把 Home 改成卡片列表；
* 自己加 Secret Match；
* 自己加题库分类；
* 自己加引导页；
* 自己修改 Challenge 文案措辞；
* 自己增加语音识别；
* 自己增加摄像头；
* 自己让 Bomb 在 Handoff 时继续倒计；
* 自己让 Round 自动跳 Challenge；
* 自己取消 Lose Reveal 的停顿；
* 自己用默认 AlertDialog；
* 自己为了复用把 Ready / Lose / Challenge 合成一个万能组件导致布局变化。

## SPEC\_GAP 协议

如果实现过程中遇到本文档没有定义的细节，开发 AI 必须输出：

```
SPEC_GAP
Area: <页面/组件>
Question: <缺失信息>
Current neutral fallback: <最小占位实现>
Visual impact: none / low / medium / high
```

### 中性占位原则

* 颜色使用已有 Token；
* 尺寸使用相邻组件规律；
* 不新增新 icon；
* 不新增新文案；
* 不新增动画；
* 不新增交互分支。

如果 `Visual impact = high`，必须停止该 UI 子项，不允许自行完成。

## 版本变更

只有产品负责人明确说“修改规格 / 冻结新版本”后才能改变本文档约束。聊天中一句临时脑暴不自动覆盖实现合同；需要同步回 GitBook 后才算正式变更。
