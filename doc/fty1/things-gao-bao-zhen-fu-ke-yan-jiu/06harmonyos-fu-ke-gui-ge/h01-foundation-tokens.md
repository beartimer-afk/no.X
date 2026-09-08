# H01｜Foundation Tokens

Foundation 是所有页面与组件唯一允许依赖的视觉基础层。

## Token 类别

```
ThingsColors
ThingsTypography
ThingsSpacing
ThingsSize
ThingsRadius
ThingsIcons
ThingsMotion
ThingsHitTarget
ThingsSafeArea
ThingsShadow
```

页面和组件禁止散落：

```ts
.fontColor('#333333')
.padding(16)
.height(52)
.borderRadius(12)
```

只能引用语义 Token。

## Token 状态

每个 Token 必须附带状态：

* `VERIFIED`：已有足够真机/Golden 证据，可冻结；
* `PROVISIONAL`：已有 C 级测量或高置信 Seed，但仍需模拟器/截图校准；
* `PENDING`：无可靠数值，不允许模型自行猜最终值。

## 单位

HarmonyOS 实现基线：

* 布局主要使用 `vp`；
* Typography 使用 `fp`；
* 不把 iPhone 录屏编码像素 epx 直接当 vp；
* epx 只作为比例和初始 Seed 来源。

HarmonyOS 平台的 8vp / 4vp 网格只作为平台参考；有 Things 真机证据时，Things 实际几何优先，不能为了网格强行把 18 改成 16、45 改成 48。

## 颜色治理

任何录屏取色都必须注明是**视频编码后的 C 级样本**，受压缩、色彩空间和录屏处理影响。

Token 记录至少包含：

```
name
value / seed
source
source frame
confidence
status
allowed tolerance
last calibrated at
```

## 字体治理

禁止直接把 iOS 字体名硬搬到鸿蒙。复刻目标是字号、字重、行高、baseline 与阅读密度，而不是文件级字体复制。

Typography Token 需要在 Visual Lab 中独立展示所有文本层级，使用固定中英文样例做截图比对。

## Hit Target

视觉尺寸与点击热区分离。类似 Checkbox 可保持很小的视觉圆圈，但外层点击目标使用更大的可触达区域。

## Safe Area

页面负责系统状态栏、导航区域和键盘避让；普通组件不得硬编码具体设备的 status bar / keyboard 高度。

## Literal Scan

工程增加静态扫描：业务 UI 中出现非白名单 literal color / spacing / fontSize / radius 时直接报告失败，防止模型绕过 Foundation。
