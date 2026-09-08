# 05｜Pack 与自定义挑战编辑器

## 1. Pack 定义

Pack 是一组可被同一套游戏引擎运行的 Challenge 集合，可附带默认规则。

Pack 不等于“场景入口”。用户可以直接 START 使用当前 Pack，也可以在 Packs 中切换。

## 2. Pack 最小模型

* id；
* name；
* subtitle（可空）；
* coverStyle；
* source：official / user / imported；
* challengeIds\[]；
* defaultDifficultyRange；
* allowedGameTypes\[]；
* secretMatchEnabled；
* nextMovePreset；
* mediaPolicy；
* createdAt / updatedAt；
* version。

第一版所有 Pack 均本地存在。

## 3. Packs 页面

默认分组：

* 当前使用；
* 官方；
* 我的。

第一版不做远程商店、不做社区、不做公开 UGC。

Pack 卡片只显示：名称、挑战数量、是否包含媒体类型、最近使用。不要在列表中展示大量标签。

## 4. Pack 详情

首屏：

* Pack 名称；
* 挑战数量；
* 开始；
* Secret Match；
* 编辑（仅用户 Pack）。

次级区域：

* 难度范围；
* Challenge 类型分布；
* 规则预设；
* 挑战列表。

## 5. 创建 Pack

创建路径：Create → 新建 Pack。

只要求输入：

1. 名称；
2. 添加至少一个 Challenge。

封面、规则、难度范围都提供默认值，不在第一步强制配置。

## 6. Challenge 编辑器

### 必填

* 挑战内容正文。

### 可选

* 类型：普通 / 限时 / 录音 / 拍照 / 录像；
* 难度：1–5；
* 时长；
* 完成方式；
* 标签；
* 权重。

编辑器默认只露出“正文 + 类型”。高级参数折叠在“更多设置”，避免现场临时创建时过慢。

## 7. 临时挑战

在 Session 中触发“临时写一个挑战”时，使用轻量编辑器：

* 一行/多行正文；
* 可选类型；
* 保存到“仅本局”。

默认不会自动写回 Pack。用户可在 Session 结束时选择“保存到 Pack”。

## 8. 导入导出

第一版可暂缓，但数据结构预留：

* manifest；
* challenges；
* assets；
* version。

未来可形成本地文件包，如 `.duopack`。导入时必须预览 Pack 名称、挑战数量和媒体引用，禁止导入即执行脚本或任意代码。

## 9. 删除与撤销

删除 Challenge / Pack 前使用轻确认；删除后提供短时 Undo。用户 Pack 的删除不影响官方 Pack。
