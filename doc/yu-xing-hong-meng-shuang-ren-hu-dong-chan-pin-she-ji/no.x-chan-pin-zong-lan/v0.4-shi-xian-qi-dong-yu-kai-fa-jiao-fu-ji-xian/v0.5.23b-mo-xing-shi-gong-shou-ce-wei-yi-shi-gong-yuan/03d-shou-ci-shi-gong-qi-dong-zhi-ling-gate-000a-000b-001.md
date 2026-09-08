# 03D｜首次施工启动指令：Gate → 000A → 000B → 001

本页给编排者复制使用。**不要一次让 3B 连做四步。** 每一步必须 STOP、人工验收、commit 后再继续。

## Step 0｜编排者准备

1. 将 `NOX_static_assets_v0.1.zip` 放到本地；
2. 校验 ZIP SHA-256：`3bc4622f8a5039ea6693120cdeeac6b17eb716f2d3ecc81db5d902d2396d943c`；
3. 解压到仓库根目录：`design_delivery/NOX_static_assets_v0.1/`；
4. 不允许 3B 自己下载这个包的替代品。

## Step 1｜只做开工 Gate

给 3B：`00 + 01 + 03A + 开工 Gate`。

```
你现在只执行 NO.X 开工 Gate。不得修改任何文件。逐条验证 G01-G07。任何 FAIL 立即 STOP。全部 PASS 时只能输出 NOX_PRECHECK: PASS 和 NEXT: TASK-000A ONLY。
```

## Step 2｜只做 TASK-000A

给 3B：`00 + 01 + 02 + 03A + 03B TASK-000A`。

```
只执行 TASK-000A。只复制字符串资源和5个运行时SVG，验证SHA-256与无外部字体。禁止接App Icon，禁止做UI。完成后STOP。
```

通过后 commit：`NOX TASK-000A resources and font freeze`

## Step 3｜只做 TASK-000B

给 3B：`00 + 01 + 02 + 03A + 03C TASK-000B`。

```
只执行 TASK-000B。只接入 HarmonyOS 双层 App Icon 与 app.json5 icon 引用。遇到 module ability icon override 必须 SPEC_GAP，禁止猜。完成后安装截图并 STOP。
```

通过后 commit：`NOX TASK-000B layered app icon`

## Step 4｜才允许 TASK-001

给 3B：`00 + 01 + 02 + 03 + 03A + 04 TASK-001`。

```
只执行 TASK-001 Home 静态页。资源和字体已经冻结，不得下载任何新素材。完成后提交 TASK001_home_390.png 并 STOP。
```

## 失败处理

出现“我找了个类似图标/字体”“先用占位图”“顺便把首页也做了”——当前 TASK 直接 FAIL，回退上一个 PASS commit，重新执行。
