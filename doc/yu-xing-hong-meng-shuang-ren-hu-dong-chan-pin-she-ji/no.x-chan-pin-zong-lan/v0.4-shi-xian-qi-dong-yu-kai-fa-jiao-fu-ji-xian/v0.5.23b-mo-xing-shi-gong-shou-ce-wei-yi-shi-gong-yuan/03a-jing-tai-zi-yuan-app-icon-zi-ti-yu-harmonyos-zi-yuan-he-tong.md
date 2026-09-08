# 03A｜静态资源、App Icon、字体与 HarmonyOS 资源合同

> 这是 3B 每个涉及 UI / 资源 TASK 的强制合同。资源缺失时停止，不允许自行下载或临时生成。

## 1. 官方 HarmonyOS 约束

NO.X App Icon 必须遵守 HarmonyOS 当前应用图标规范：

* 图标必须使用**分层资源**；
* 手机/折叠屏/平板使用前景 + 背景双层；
* 每层画布 `1024×1024px`；
* PNG；
* 源图为正方形，**不要手工做圆角**，圆角/遮罩由系统处理；
* 背景层不得有透明像素；
* 上架前需验证不同系统场景下的小尺寸识别。

官方参考：

* https://developer.huawei.com/consumer/cn/doc/doccenter-ux-design/application-icon-0000001953444009
* https://developer.huawei.com/consumer/en/doc/harmonyos-guides-V5/layered-image-V5
* https://developer.huawei.com/consumer/cn/design/resource/

## 2. 唯一设计侧资源包

文件名固定：`NOX_static_assets_v0.1.zip`

编排者在施工前必须把它解压到仓库根目录：

`design_delivery/NOX_static_assets_v0.1/`

ZIP SHA-256：

`3bc4622f8a5039ea6693120cdeeac6b17eb716f2d3ecc81db5d902d2396d943c`

3B 不得使用同名以外资源包，不得从网络补齐。

## 3. 资源包固定内容

### App Icon 正式运行时资源

```
AppScope/resources/base/media/
├── nox_icon_background.png
├── nox_icon_foreground.png
└── nox_app_icon.json
```

要求：background 1024×1024 且全部 alpha=255；foreground 1024×1024透明背景。

`nox_app_icon.json` 固定为：

```json
{
  "layered-image": {
    "background": "$media:nox_icon_background",
    "foreground": "$media:nox_icon_foreground"
  }
}
```

`app.json5` 的 `app.icon` 必须引用：`$media:nox_app_icon`。

### App Icon 视觉来源

foreground 已由设计侧从**已确认的 NO.X 霓虹紫粉 X 形科技徽章视觉**整理成透明前景层。开发模型只允许使用该文件，不允许重新画 X、重新生成图标或把视觉简化成系统字体 X。

### UI 运行时图标

```
entry/src/main/resources/base/media/
├── nox_ic_back.svg
├── nox_ic_close.svg
├── nox_ic_settings.svg
├── nox_ic_heart.svg
└── nox_ic_flame.svg
```

全部为 NO.X 自制线性 SVG。开发模型不得从第三方图标库换图。

### 设计母版

```
design_assets/icon/
├── nox_app_icon_master_1024.png
└── nox_app_icon_safearea_1024.png

design_assets/brand/
└── nox_mark_master_1024.png
```

母版只用于人工比对，不得整张切片进 App。

### 多语言 App 名称

`app_name` 固定 `NO.X`；中文 tagline `两个人，一个未知。`；英文 `Two players. One unknown.`。

## 4. App Icon 视觉冻结

唯一方向：**深黑/暗紫底 + 霓虹粉紫圆环 + 中心 X 能量标记**。

禁止：卡通炸弹、嘴唇、情侣照片、爱心炸弹、系统字体单独 X、完整 NO.X 四字符塞入小图标、金色奢华风、蓝绿色赛博风。

开发模型没有 App Icon 设计权，只能复制和配置资源包现成文件。

## 5. 字体合同

v0.1 **不随 App 分发任何 `.ttf` / `.otf` 字体文件**。

* 不创建 `fonts/`；
* 不下载 Google Fonts / 免费商用字体 / 赛博字体；
* 不声明第三方 fontFamily；
* UI 使用 HarmonyOS/设备系统无衬线字体；
* 品牌感通过字号、字重、字间距、颜色和布局完成。

官方设计资源提供 HarmonyOS Sans 参考资源，但 v0.1 直接使用系统字体，不把字体文件打包进项目。

## 6. 背景与图片资源

v0.1 默认不依赖人物摄影。Home / Ready / Bomb / Lose / Challenge 使用代码绘制的纯色、渐变、Bloom、Glow、Card、Core。

禁止网络下载情侣/成人摄影、自行AI生成临时人物图、水印素材、视觉探索截图切片、图库依赖。

## 7. 声音

资源包当前不含音频。TASK-014 之前只打声音事件日志；TASK-014 默认静默/no-op，不得自行下载 wav/mp3。

## 8. 文件校验

资源包内存在 `SHA256SUMS.json`。TASK-000A / 000B 复制前后必须逐文件 SHA-256 校验；不一致即 FAIL。

## 9. 资源缺失唯一输出

```
STATUS: BLOCKED
BLOCK_ASSET_MISSING:
- <缺失文件>
NEXT ACTION: STOP
```

禁止“先放占位图以后再换”。
