# 03C｜TASK-000B：HarmonyOS 分层 App Icon 接入

## 目标

将 NO.X 正式分层 App Icon 接入 HarmonyOS 工程，替换默认图标。只做图标资源与 app 配置，不做业务 UI。

## 依赖

TASK-000A PASS。

## 允许修改

```
AppScope/resources/base/media/nox_icon_background.png
AppScope/resources/base/media/nox_icon_foreground.png
AppScope/resources/base/media/nox_app_icon.json
AppScope/app.json5
```

如果当前工程 app.json5 实际路径不同，只允许修改**现有 AppScope app.json5**，不得新建第二份配置。

## Step A｜图标文件

1. 从 `design_delivery/NOX_static_assets_v0.1/AppScope/resources/base/media/` 读取三个源文件；
2. 目标同名文件不存在：原样复制；
3. 目标同名文件存在：先 SHA-256；与源一致则保留；不一致则 `SPEC_GAP: APP_ICON_RESOURCE_COLLISION`，STOP，禁止覆盖；
4. 复制后与源包 `SHA256SUMS.json` 对照，三项完全一致；
5. foreground/background 均必须 1024×1024 PNG；
6. background 每个像素 alpha=255；任何透明像素=FAIL；
7. 禁止裁圆角、缩放、再编码、加透明边距。

## Step B｜app.json5 只允许改一个字段

先完整读取现有 app.json5。

只允许把 `app.icon` 的值设置为：

`$media:nox_app_icon`

除 `app.icon` 外，app.json5 **任何已有字段和值都必须保持不变**。

### alternateIcons

* 若字段不存在：禁止新增；
* 若字段已存在：不要删除/修改，输出 `SPEC_GAP: PREEXISTING_ALTERNATE_ICONS`，STOP 等人工决定。

### module ability icon override

搜索 module.json5 的 `abilities[].icon`：

* 不存在：继续；
* 存在：不修改 module.json5，输出 `SPEC_GAP: MODULE_ABILITY_ICON_OVERRIDE` + 当前值，STOP。

## Step C｜构建与安装

1. Debug build；
2. 安装到模拟器/真机；
3. 截图证明桌面/应用列表显示 NO.X 正式图标；
4. STOP。

## 视觉验收

必须看到：暗黑/暗紫底、粉紫霓虹圆环、中心 X。小尺寸仍可识别 X + Ring。

禁止：DevEco 默认图标、白底、系统蓝、卡通炸弹、完整文字 NO.X 堆叠、人工圆角形成双重圆角。

## 必交

```
TASK000B_icon_desktop.png
TASK000B_icon_hash_check.txt
TASK000B_app_json5_before_after.txt
```

before/after 证明除 `app.icon` 外无其他 app.json5 字段变化。

## 验收

A01双层1024 PNG；A02背景无透明；A03三资源哈希正确；A04 app.icon准确；A05 app.json5其余字段零变化；A06无默认图标；A07未修改页面/module配置；A08 Debug build PASS。
