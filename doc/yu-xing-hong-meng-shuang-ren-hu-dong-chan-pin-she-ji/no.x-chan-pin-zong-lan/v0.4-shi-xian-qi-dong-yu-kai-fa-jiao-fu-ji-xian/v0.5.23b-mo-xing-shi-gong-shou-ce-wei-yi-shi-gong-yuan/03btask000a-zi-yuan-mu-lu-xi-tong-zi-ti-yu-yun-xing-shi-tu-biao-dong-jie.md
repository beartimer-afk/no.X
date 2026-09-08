# 03B｜TASK-000A：资源目录、系统字体与运行时图标冻结

## 目标

只把设计侧资源包中的**字符串 key + 5 个运行时 SVG 图标**按固定规则接入工程，并冻结字体策略。本任务不接 App Icon，不做页面 UI。

## 依赖

* 开工 Gate 全 PASS；
* `design_delivery/NOX_static_assets_v0.1/` 存在；
* `resource_manifest.json` 与 `SHA256SUMS.json` 可读。

缺任一项：BLOCK，STOP。

## 允许修改

```
AppScope/resources/base/element/string.json
AppScope/resources/zh_CN/element/string.json
AppScope/resources/en_US/element/string.json
entry/src/main/resources/base/media/nox_ic_back.svg
entry/src/main/resources/base/media/nox_ic_close.svg
entry/src/main/resources/base/media/nox_ic_settings.svg
entry/src/main/resources/base/media/nox_ic_heart.svg
entry/src/main/resources/base/media/nox_ic_flame.svg
```

若目录不存在，可创建上述目录。禁止修改 ETS 页面。

## Step A｜5 个 SVG 的唯一处理方式

1. 读取源包 `SHA256SUMS.json`；
2. 确认 5 个源 SVG 存在；
3. 若目标文件不存在：原样复制；
4. 若目标同名文件已经存在：先计算 SHA-256；
   * 与源包相同：保留，视为 PASS；
   * 与源包不同：`SPEC_GAP: STATIC_ICON_NAME_COLLISION <path>`，STOP，**禁止覆盖**；
5. 复制后再次 SHA-256 校验。

## Step B｜string.json 禁止整文件覆盖

每个语言目录分别执行相同合并算法：

### 若目标 string.json 不存在

允许创建文件，内容使用资源包版本。

### 若目标 string.json 已存在

必须 parse JSON，并且只处理以下两个 `name`：

* `app_name`
* `home_tagline`

规则：

1. 保留目标文件所有其他 string entry，不删除、不改值、不改 name；
2. `app_name` 已存在则只把 value 改成 `NO.X`；不存在则追加；
3. `home_tagline`：
   * base / zh\_CN = `两个人，一个未知。`
   * en\_US = `Two players. One unknown.` 已存在则只改 value，不存在则追加；
4. 禁止把资源包 string.json 整体覆盖目标文件；
5. 禁止重命名用户其他资源 key；
6. JSON 必须保持合法，构建可读。

因此 string.json **不要求与源包整文件 SHA-256 相同**；只验收指定 key 的最终值。

## Step C｜字体冻结

NO.X v0.1 使用 HarmonyOS / 设备系统无衬线字体。

本 TASK 新增文件中：`.ttf = 0`、`.otf = 0`、新 `fonts/` 目录 = 0。

若仓库在任务前已经存在第三方字体文件：不得删除、不得引用；输出 `SPEC_GAP: PREEXISTING_FONT_ASSET` 并列路径，STOP 等人工决定。

## Step D｜构建

Debug build，输出变更文件列表。

## 禁止

* 禁止修改 app.json5；
* 禁止接 App Icon；
* 禁止创建 Home；
* 禁止换 SVG path/颜色；
* 禁止下载/挑选其他 icon；
* 禁止整文件覆盖已有 string.json；
* 禁止删除任何已有资源。

## 必交

```
TASK000A_resource_tree.txt
TASK000A_icon_hash_check.txt
TASK000A_string_merge_check.txt
```

回复必须显示：`NO_EXTERNAL_FONTS: PASS`。

## 验收

A01 5个 SVG 齐全且哈希正确；A02 三个语言文件指定 key 正确；A03 原有字符串零丢失；A04 无新字体；A05 无第三方资源；A06 未修改 ETS/app.json5；A07 Debug build PASS。
