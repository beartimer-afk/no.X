# 14｜【HarmonyOS UI / Interaction 补充说明｜Personal Storage Layer / Mobile Photos V1】

**邮件主题：** 【HarmonyOS实施补充】Personal Storage Layer — Mobile Photos V1｜UI/交互规范与验收补齐

**收到时间：** 2026-09-06T04:45:09Z

## 邮件正文原文

```
【HarmonyOS UI / Interaction 补充说明｜Personal Storage Layer / Mobile Photos V1】  
  
结论  
在不修改现有 V1 产品逻辑、Domain、Protection Policy、Plugin Protocol、Local-only / Private
边界和 AT-01～AT-35 的前提下，已依据 2026 年 HarmonyOS 官方设计规范补齐鸿蒙平台的 UI /
交互实施规则。本邮件作为“鸿蒙实施附录”，低成本 AI 在实现 HarmonyOS 版本时必须同时遵守原《正式开发冻结规格》和本补充说明。  
  
一、官方规范依据  
  
1\. HarmonyOS Design  
<https://developer.huawei.com/consumer/cn/design>  
  
2\. Layout Basics（vp / fp / 8vp Grid / Safe Area / Breakpoint / Responsive
Layout）  
<https://developer.huawei.com/consumer/cn/doc/doccenter-ux-design/design-
layout-basics-0000001795579413>  
  
3\. List 设计规范（列表层级、点击态、选中态、不可用态、焦点态）  
<https://developer.huawei.com/consumer/cn/doc/doccenter-ux-
design/list-0000001929853910>  
  
4\. HarmonyOS Sans  
<https://developer.huawei.com/consumer/cn/doc/doccenter-ux-
design/font-0000001828772001>  
  
5\. SubHeader  
<https://developer.huawei.com/consumer/cn/doc/doccenter-ux-
design/subheader-0000001929816012>  
  
6\. Dialog Overview  
<https://developer.huawei.com/consumer/cn/doc/doccenter-capabilities/arkts-
dialog-overview>  
  
7\. 模态 / 非模态弹窗焦点语义  
<https://developer.huawei.com/consumer/cn/doc/doccenter-capabilities/pop-up-
controls-focus>  
  
8\. bindSheet / 半模态  
<https://developer.huawei.com/consumer/cn/doc/doccenter-dev-faq/faqs-
arkui-1039>  
  
9\. 2026 UX 变化：Rounded Rectangle Button、Sheet Safe Area  
<https://developer.huawei.com/consumer/cn/doc/doccenter-release-
notes/changelogs-ux-5101>  
  
二、HarmonyOS 实施总原则  
  
1\. 使用 HarmonyOS 原生设计语言和系统组件优先，不仿 iOS / Android 控件。  
2\. 保留“Home 为唯一一级入口”，不得因适配鸿蒙新增底部导航。  
3\. 使用 HarmonyOS Sans。  
4\. 布局使用 vp，文字使用 fp；主要间距基于 8vp Grid。  
5\. 所有界面必须避让系统安全区、状态栏、挖孔区、底部系统区域。  
6\. 手机、折叠屏展开态、平板/宽屏窗口变化时使用响应式布局，不写死手机尺寸。  
7\. 动效只用于保持状态连续性，不得阻塞业务状态写入。  
8\. 高风险不可逆动作使用模态 Dialog；普通选择、保护方式、缺 Storage 等使用半模态/页面内反馈。  
9\. 无障碍、字体缩放、焦点和选中态进入 V1 验收。  
  
三、Home 页面  
  
Home 仍然是唯一一级页面。  
  
建议使用 HarmonyOS Navigation / 标题栏体系：  
\- 一级 Home 不显示返回按钮。  
\- 右侧仅保留 Storage/Settings，以及仅在有问题时出现的 Problems 入口。  
\- 不加入搜索、账户头像、分享等本期范围外入口。  
  
Photo Feed：  
\- 使用 Grid / 等价响应式网格，不用 List 模拟照片墙。  
\- 时间倒序、按日期分组。  
\- 列数根据可用宽度自适应。  
\- Standard 正常状态不显示常驻复杂状态。  
\- Important / Private / Pending / Problem 使用轻量 Overlay，但不得遮住照片主体。  
\- 选中态必须使用勾选、轮廓、遮罩等明确反馈，不允许只靠颜色。  
  
四、权限 / 空状态  
  
无权限时：  
\- 在 Home 页面内原位引导。  
\- 不弹自制强制模态。  
\- 不要求先配置 Storage。  
\- 不出现 Login / Register / Onboarding Carousel。  
  
如果最终 HarmonyOS 的系统 Picker 可以满足真实持续图库场景，实施时优先使用系统 Picker /
系统授权能力；是否能满足持续读取需求属于平台能力实现 Gate，需要编码前按当前 HarmonyOS API 实际验证。  
  
Partial Access：  
\- 照常显示当前可见照片。  
\- 顶部用非阻断提示说明“当前只显示已允许访问的照片”。  
\- 不反复弹 Dialog。  
  
五、Selection Mode  
  
长按进入多选。  
  
Top Bar：  
Cancel | 已选择 N 项  
  
Bottom Action：  
唯一核心动作“设置保护方式”。  
  
选中态：  
\- 必须有清晰视觉反馈。  
\- Accessibility 必须可读出“已选择/未选择”。  
\- 高频底部操作放在拇指容易操作的位置。  
\- 宽屏时底部操作区不应无限横向拉伸。  
  
六、Protection Picker  
  
HarmonyOS 上采用半模态 Sheet / bindSheet 等价形态，不使用全屏新页面，也不使用 AlertDialog 做普通四项选择。  
  
标题：  
“如何保护这些照片？”  
“已选择 N 项”  
  
选项：  
Standard｜普通  
Important｜重要  
Private｜私密  
Private + Important｜私密且重要  
  
交互：  
\- 点击选项立即确认。  
\- 关闭 Sheet 等价于 Cancel。  
\- Cancel 后返回 Selection，并保留已选照片。  
\- Sheet 高度优先内容自适应。  
\- 最大高度避让状态栏/安全区。  
\- 折叠屏/宽屏跟随 HarmonyOS 半模态系统形态自适应，不强制全宽手机底部大面板。  
  
七、Storage Required Prompt  
  
这是“用户保护意图已保存，但缺依赖”，不是高风险确认。  
  
使用半模态 Sheet：  
标题：“需要配置存储”  
Primary：“配置存储”  
Secondary：“稍后”  
  
必须先保存 ProtectionAssignment，再显示该 Sheet。  
“稍后”不得撤销 Assignment。  
  
八、Storage Center  
  
使用标准 List + SubHeader。  
  
分组：  
“已配置”  
“待设置”  
“添加存储”  
  
Storage Instance Item：  
\- Plugin icon  
\- Display name  
\- Provider subtitle  
\- 状态文本  
\- 详情入口  
  
状态必须“图标 + 文本”，不能只依赖颜色：  
Ready → 可用  
AUTH_EXPIRED → 需要重新连接  
UNAVAILABLE → 暂不可用  
DRAFT → 完成设置  
  
九、Plugin Store  
  
使用原生 List / Card，而不是 WebView 商店。  
  
顶部：  
标题“插件”  
Search  
  
卡片：  
Icon  
Name  
Publisher  
一句话用途  
费用标签  
Installed 状态  
  
Marketplace Error：  
\- 页面内错误 + Retry。  
\- 不使用阻断型 Dialog。  
\- 不影响 Home、已安装 Plugin 和已有 StorageInstance。  
  
十、Plugin Detail  
  
普通二级 Navigation 页面。  
  
必须按顺序包含：  
1\. Header：Icon / Name / Publisher / Install  
2\. What it does  
3\. Good for  
4\. Data access  
5\. Privacy  
6\. Pricing  
7\. Capabilities  
8\. Version  
  
按钮使用 HarmonyOS 当前系统 Rounded Rectangle 风格优先，不自行固定为旧式 Capsule。  
  
Pricing 必须分：  
\- Plugin 费用  
\- Provider 第三方费用  
  
十一、Plugin Setup / 表单  
  
Core 渲染统一的原生表单。  
  
字段具备：  
Label  
Input / Selector  
Helper text  
Validation  
Error  
  
Secret：  
\- 默认遮蔽。  
\- 可显隐。  
\- 错误信息不得回显。  
\- 日志不得记录。  
\- Accessibility 不得朗读明文 secret。  
  
“Test Connection”：  
只有结构校验通过时可用。  
  
字段错误使用 inline 提示，不为普通表单错误弹 Dialog。  
  
十二、Capability Test  
  
必须采用逐项步骤 UI，不使用一个总 spinner。  
  
显示：  
Authentication  
Target  
Write  
Stat  
Verify  
  
每项状态：  
Waiting  
Testing  
Passed  
Failed  
Unsupported  
  
必须图标 + 文本，不只使用红绿颜色。  
  
Only Auth PASS 不代表 Ready。  
只有满足 Policy 所需能力时 StorageInstance 才 Ready。  
  
十三、Problems  
  
Home 只有存在 NEEDS_ATTENTION / DEGRADED 时显示 Problems 入口。  
  
Problems 页面使用 List + SubHeader，并按用户修复动作分组：  
配置存储  
重新连接  
修复配置  
保护未完成  
恢复加密访问  
重试  
  
仍在自动 retry 的临时网络错误不要频繁打扰。  
  
十四、Dialog 使用规则  
  
必须用模态 Dialog：  
\- Remove StorageInstance 且会影响保护状态。  
\- Discard 未保存配置。  
\- 明确 Delete remote data。  
\- 其他不可逆高风险动作。  
  
不要用模态 Dialog：  
\- Protection Picker。  
\- Storage Required。  
\- 普通成功。  
\- 临时 Retry。  
\- Marketplace 加载失败。  
\- 普通 Capability Test Fail。  
  
危险确认文案不能只写“确定/取消”，必须直接写后果和动作。  
  
十五、按钮、热区、字体  
  
\- 使用 HarmonyOS 系统 Button。  
\- 新规范优先 Rounded Rectangle。  
\- 小图标按钮必须确保合理触控热区，必要时扩大 response region。  
\- Disabled 状态清晰。  
\- 关键动作不能只藏在长按 Context Menu。  
\- 使用 HarmonyOS Sans。  
\- 文本用 fp，支持系统字体缩放。  
\- 大字号情况下不得裁切关键按钮/说明。  
\- 不引入第三方 UI 字体。  
  
十六、布局 / 响应式  
  
使用：  
vp  
fp  
8vp Grid  
Safe Area  
Breakpoint  
Responsive Layout  
  
必须验证：  
\- 手机竖屏  
\- 手机横屏（若具体版本支持）  
\- 折叠屏展开态  
\- 平板/宽屏  
\- 分屏尺寸变化  
\- 字体放大  
  
照片 Grid：  
宽度变化优先增加/减少列数，不把单个图片无限放大。  
  
二级详情：  
宽屏可增加最大内容宽度或信息分栏，但不得改变业务层级。  
  
十七、动效  
  
使用 HarmonyOS 原生自然动效优先。  
  
需要状态连续性的地方：  
\- 进入/退出 Selection  
\- Protection Picker Sheet  
\- Plugin Install  
\- Capability Test  
\- Pending → Protected  
\- Problems 数量变化  
  
规则：  
\- 动效不阻塞点击。  
\- 状态先写 Domain，再表现动效。  
\- 远端任务不依赖动画生命周期。  
\- 平台支持减少动画时应尊重系统设置。  
  
十八、无障碍要求  
  
V1 必须增加鸿蒙无障碍验收：  
1\. 照片选中态可被屏幕朗读识别。  
2\. Icon-only Button 有 accessibility label。  
3\. Private / Important / Pending / Problem 不只依赖颜色。  
4\. 模态 Dialog 打开后焦点进入 Dialog。  
5\. Dialog 关闭后焦点回到合理原位置。  
6\. 非模态提示不锁死焦点。  
7\. 字体放大不截断主操作。  
8\. Secret 不被朗读明文。  
9\. Test 每一步状态有文本语义。  
10\. Problems 每项能朗读“问题 + 修复动作”。  
11\. 支持键盘/焦点设备形态时，列表/按钮焦点顺序合理。  
  
十九、新增 HarmonyOS Acceptance Tests  
  
HAT-01  
Home 仍是单一级 Navigation，不新增底部 Tab。  
  
HAT-02  
布局使用 vp/fp 和 8vp Grid，安全区不遮挡关键内容。  
  
HAT-03  
Photo Grid 随窗口宽度自适应列数，无横向溢出。  
  
HAT-04  
Selection 选中态不只靠颜色，Accessibility 可识别。  
  
HAT-05  
Protection Picker 使用 HarmonyOS 半模态/等价轻量面板，不用全屏页。  
  
HAT-06  
Storage Required 使用非破坏性半模态，关闭后 Assignment 仍在。  
  
HAT-07  
Remove/Delete 等高风险动作使用模态 Dialog，普通失败不滥用 Dialog。  
  
HAT-08  
Plugin Detail 主按钮使用系统 Rounded Rectangle 风格；字体放大可正常显示。  
  
HAT-09  
Capability Test 逐项展示，不允许只有总 spinner。  
  
HAT-10  
Secret 默认遮蔽，并通过日志/错误/无障碍测试证明不泄露。  
  
HAT-11  
Marketplace Error 可在页面内 Retry，不影响已安装插件。  
  
HAT-12  
字体缩放后 Home / Picker / Plugin Setup / Problems 不遮挡主操作。  
  
HAT-13  
折叠屏/宽屏时内容和 Sheet 自适应，不强制手机固定宽度。  
  
HAT-14  
所有 icon-only controls 有 Accessibility Label 和合理点击热区。  
  
HAT-15  
模态 Dialog 的焦点进入/退出行为正确。  
  
HAT-16  
Private / Important / Pending / Problem 均存在非纯颜色表达。  
  
二十、对原正式开发规格的关系  
  
本邮件是 HarmonyOS 平台实施附录，不修改原本冻结的：  
\- Home 唯一一级入口  
\- Local-only  
\- 无我们的账号体系  
\- Important / Private 正交  
\- Private 本地真加密  
\- Plugin 最小权限  
\- Upload != Verified != Protected  
\- GitHub 仅存在 Plugin 内  
\- Recovery Kit 用户自己持有  
\- Marketplace 不获取用户数据  
\- AT-01～AT-35  
  
开发 AI 在 HarmonyOS 实施时必须同时满足原 AT-01～AT-35 + 本邮件 HAT-01～HAT-16。  
  
若某个 HarmonyOS 系统组件限制与本邮件具体组件形式冲突，允许替换组件实现，但不允许改变产品语义、Domain 或 User
Flow。任何需要改变核心产品逻辑的冲突必须先报告。  
  
【结论】  
HarmonyOS UI / 交互适配补充完成。当前规格已经可以进入 ArkUI 的具体页面、组件和状态绑定实现阶段。


```
