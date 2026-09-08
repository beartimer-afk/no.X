# 07｜【T05 完成｜V1 页面与交互规格】

**邮件主题：** RE: 【项目立项】Personal Storage Layer — Mobile Photos V1｜T01–T10 主线程

**收到时间：** 2026-09-05T16:15:08Z

## 邮件正文原文

```
【T05 完成｜V1 页面与交互规格】  
  
验收结论：PASS  
  
一、目标  
把 T01–T04 的产品与状态机落实成低成本 AI 可直接实现的
UI/交互规格。当前不绑定具体平台设计语言，因此只冻结信息架构、组件层级、交互规则、状态反馈和文案语义；视觉像素、平台控件映射属于后续实施阶段。  
  
二、V1 信息架构  
产品只有一个一级主页面：Home。  
  
不存在底部多 Tab。  
不存在首次 Onboarding Carousel。  
不存在我们的 Login/Register。  
不存在“先设置再使用”。  
  
二级/临时界面：  
1\. Protection Picker（从 Home Selection 触发）  
2\. Storage Required Prompt  
3\. Storage Center  
4\. Plugin Store  
5\. Plugin Detail  
6\. Plugin Activation / Configuration  
7\. Capability Test Result  
8\. Problems / Protection Details  
9\. Storage Instance Detail  
  
所有二级界面最终都可回 Home。  
  
三、Home｜主页面  
  
目标：  
打开 App 第一眼就是用户照片，而不是产品功能。  
  
结构：  
A. Top Bar  
\- 左侧/中心：产品标题可弱化，V1 不要求品牌大标题。  
\- 右侧：Storage/Settings 入口（一个统一入口即可）。  
\- 若存在需要用户处理的问题，可在 Top Bar/顶部区域出现 Problem Indicator。  
  
B. Photo Feed  
\- 时间倒序，最近照片优先。  
\- 按日期分组：Today / Yesterday / 日期。  
\- 采用紧凑照片网格/瀑布流。  
\- 图片是绝对视觉主体。  
\- Standard 且无异常的照片不显示常驻文字。  
  
C. Status Overlay 原则  
只对“有意义的例外”显示轻量状态：  
\- Important：小型 Important 标识。  
\- Private：Private 标识；具体是否模糊缩略图不在 V1 默认开启，避免把 Private 与 Hidden 混为一谈。  
\- Private+Important：可组合或使用单一复合标识。  
\- PENDING：极轻量进度/等待标识。  
\- NEEDS_ATTENTION / DEGRADED：明显但不遮挡照片主体的问题标识。  
\- PROTECTED：默认不要求每张图永久显示大号 ✓；可在详情/短暂反馈/筛选状态中体现，减少视觉噪音。  
  
四、Home 空/权限状态  
  
HOME_ACCESS_UNKNOWN  
主区域：  
标题：查看并保护你的照片  
说明：允许访问照片后，它们会显示在这里。照片和保护设置保存在本地。  
主按钮：选择/允许访问照片  
  
禁止：  
\- 登录按钮  
\- 先添加 Storage  
\- 先解释 5 页产品理念  
  
HOME_ACCESS_DENIED  
标题：无法显示照片  
说明：当前没有照片访问权限。  
主按钮：管理照片访问  
次要：继续留在空 Home  
  
HOME_ACCESS_PARTIAL  
照片正常显示。  
顶部非阻断提示：  
“当前只显示已允许访问的照片。”  
动作：管理访问  
提示可关闭，但状态入口仍可从设置访问。  
  
HOME_EMPTY  
标题：这里还没有照片  
说明：当有可访问照片时会显示在这里。  
不引导 Storage。  
  
HOME_LOADING  
若没有缓存：骨架网格。  
若已有内容：保留已有照片，只对增量区域显示加载。  
禁止全屏 spinner 阻断已有浏览。  
  
HOME_LIBRARY_ERROR  
标题：暂时无法读取照片  
动作：重试  
不得显示为 Empty。  
  
五、Selection Mode  
  
进入：  
用户长按/执行平台等价选择动作选择第一张照片。  
  
变化：  
Top Bar 切换：  
左：Cancel  
中：已选择 N 项  
右：可留空；不塞分享/删除等无关动作。  
  
Photo Feed：  
\- 被选中项必须有明确选中态。  
\- 点击未选项→加入。  
\- 点击已选项→移除。  
\- 连续范围选择作为产品语义支持，具体手势后置。  
  
Bottom Action Bar：  
V1 只保留一个主要动作：  
“设置保护方式”  
或更短：“保护”  
  
原因：  
避免低成本 AI自行加入 Share/Delete/Album 等相册功能。  
  
选择数变 0：  
自动退出 Selection。  
  
Cancel：  
清空选择，回 Browse。  
  
六、Protection Picker  
  
呈现：  
从底部/上下文弹出的临时选择层，不跳独立全屏页面。  
  
标题：  
“如何保护这些照片？”  
副标题：  
“已选择 N 项”  
  
四个选项：  
1\. Standard｜普通  
说明：使用默认保存方式。  
  
2\. Important｜重要  
说明：按重要内容策略保留经过验证的远端副本。  
  
3\. Private｜私密  
说明：上传前先在本地加密。  
  
4\. Private + Important｜私密且重要  
说明：本地加密，并按重要内容策略保存。  
  
视觉：  
不要做“安全等级进度条”。  
不要 Low/Medium/High。  
四项不是从弱到强的线性等级。  
  
已选照片若 Mark 混合：  
Picker 初始显示 Mixed / 未统一，不擅自选某一项。  
用户选择后批量覆盖为该组合。  
  
Cancel：  
回 Selection，保留选择。  
  
提交：  
点击某选项即视为确认，不再二次“Save”。  
立即持久化 Assignment。  
  
七、打标后的即时反馈  
  
场景 A：无需远端动作/当前已满足  
Picker 关闭→Selection 结束→Home。  
短暂非阻断反馈：  
“已更新 N 项”。  
  
场景 B：有 READY Storage，可自动执行  
Picker 关闭→Home。  
对应资产进入 PENDING。  
顶部可出现聚合反馈：  
“正在保护 N 项”  
不弹全屏进度页。  
  
场景 C：缺 Storage  
Assignment 先保存。  
弹 Storage Required Prompt。  
  
八、Storage Required Prompt  
  
标题：  
“需要配置存储”  
  
正文根据 Mark 动态说明事实：  
Important：  
“这些照片已标记为重要，但还没有可用于重要内容策略的存储。”  
  
Private：  
“这些照片已标记为私密。需要配置存储后，系统会先在本地加密，再上传密文。”  
  
Private+Important：  
合并说明，不使用“最高安全”。  
  
按钮：  
Primary：配置存储  
Secondary：稍后  
  
配置存储：  
进入 Storage Center / Plugin Store，并保留 ResumeContext。  
  
稍后：  
回 Home。  
照片保留 Mark，并显示 Needs Attention。  
绝不撤销用户刚才的选择。  
  
九、Storage Center  
  
入口：  
Home 右上统一设置/存储入口；  
或 Storage Required Prompt。  
  
目的：  
用户查看“我已经能用哪些存储”和进入 Plugin Store。  
  
结构：  
A. 已配置存储  
每个 StorageInstance 卡片：  
\- Plugin icon/name  
\- 用户给实例的显示名；若无则 Provider 默认名  
\- 状态：Ready / Needs Reconnect / Unavailable / Needs Setup  
\- 极简 capability/status 摘要  
  
B. Installed but not configured  
已安装插件但无 READY Instance：  
显示“完成设置”。  
  
C. Add Storage  
主动作：“添加存储”  
→ Plugin Store  
  
D. Plugin Store 入口  
可与 Add Storage 合并。  
  
禁止：  
把 Marketplace 作为 Home 底部 Tab。  
  
十、Plugin Store  
  
结构：  
Top：标题“插件”  
Search Field：搜索插件名/Provider/能力关键词。  
Sections：  
\- Recommended / Available  
\- Installed（可通过状态标识，不必独立 Tab）  
  
Plugin Card：  
\- Icon  
\- Plugin name  
\- Publisher  
\- 一句话用途  
\- 费用标签：Free / Paid plugin / Provider charges may apply（按 Catalog）  
\- 状态：Installed（如已安装）  
  
点击整卡→Plugin Detail。  
不在卡片直接要求 Credential。  
  
Marketplace Error：  
保留页面框架。  
说明“暂时无法加载插件目录”。  
Retry。  
已安装插件仍从 Storage Center 可用。  
  
十一、Plugin Detail  
  
必须包含：  
  
Header  
\- Icon  
\- Plugin name  
\- Publisher  
\- Install/Installed 按钮  
  
What it does  
一句话+简短能力说明。  
  
Good for  
事实型适用场景，不写绝对安全宣传。  
  
Data access  
明确插件为了工作可能访问哪些本地/Provider 数据。  
V1 必须有这一节，即使 GitHub Plugin 很简单。  
  
Privacy  
说明：  
\- Core 数据本地保存。  
\- Private 内容上传前由 Core 本地加密。  
\- 插件/Provider 得到什么由实际策略和插件能力决定。  
  
Pricing  
严格分：  
1\. Plugin price  
2\. Provider charges  
第三方费用文案：  
“存储、流量、请求或账户费用可能由服务提供商收取。实际费用以服务提供商为准。”  
  
Capabilities  
面向用户友好展示：  
Connect  
Upload  
Verify  
以及 T06 定义的能力。  
  
Version / Publisher  
版本、发布者。  
  
Primary action：  
未安装：Install  
已安装无实例：Set Up  
已安装且有实例：Add Another / Manage（按上下文）  
  
十二、Install 交互  
  
点击 Install：  
按钮进入 Installing，防重复点击。  
  
成功：  
按钮变 Installed。  
若用户来自 Storage Required：  
自动进入 Set Up。  
若普通浏览进入：  
显示 Primary：Set Up。  
  
失败：  
页面内联错误：  
“安装失败”  
原因若可解释则显示分类。  
按钮 Retry。  
不清空 Marketplace/ResumeContext。  
  
十三、Plugin Activation / Configuration  
  
这是插件提供 schema、Core 渲染的配置页面；V1 不让插件完全自绘任意 UI，以保证一致性和权限边界。  
  
页面：  
Title：Set up [Plugin]  
  
表单字段：  
由 Plugin Configuration Schema 定义。  
每字段必须有：  
\- label  
\- type  
\- required  
\- helper text  
\- validation  
\- secret flag（若为 Credential）  
  
Secret 字段：  
默认遮蔽。  
不在错误信息、日志、确认页重复明文。  
  
Primary：  
“Test Connection”  
仅当结构校验通过可点击。  
  
Secondary：  
Cancel / Back。  
  
退出有已修改未保存配置：  
Prompt：  
Save Draft  
Discard  
Cancel  
具体平台控件后置，三种语义必须存在。  
  
十四、Capability Test UI  
  
点击 Test Connection：  
进入同页测试状态或轻量测试页，不要求全屏动画。  
  
展示测试步骤：  
Authentication  
Access  
Write  
Verification  
  
每项状态：  
Waiting  
Testing  
Passed  
Failed  
Not supported（仅非 Policy 必需能力可接受）  
  
全 PASS 且满足目标 Policy：  
标题：“Storage ready”  
Primary：  
若来自 ResumeContext：“Continue”  
否则：“Done”  
  
Continue：  
不让用户重新选择照片。  
Core 自动 Re-evaluate 原 Assignment。  
  
Partial：  
标题：“Connected, but not ready for this protection”  
列出缺少能力。  
动作：  
Edit configuration / Try again / Choose another plugin。  
  
Auth Fail：  
“Authentication failed”  
动作：Edit credentials。  
  
Network/Provider：  
“Provider unavailable”  
动作：Retry。  
不清空表单。  
  
十五、GitHub Reference Plugin 的产品配置页面（V1 示例）  
注意：具体 GitHub API 技术实现不在本任务。  
  
页面字段至少产品层表达：  
\- GitHub authorization / credential  
\- Repository target  
\- Optional base path / folder  
\- Storage instance display name  
  
Test 后显示：  
Authenticated  
Repository accessible  
Write test passed  
Verification test passed  
  
如果 Repository 不可写：  
不能 Ready。  
  
如果 Credential 有效但目标配置错误：  
提示“Repository cannot be used with this configuration”，而不是笼统 Connection Failed。  
  
十六、Problems 入口  
  
出现条件：  
存在 NEEDS_ATTENTION 或 DEGRADED PhotoAsset/StorageInstance。  
  
Home 顶部：  
聚合条/入口，例如：  
“3 项需要处理”  
  
点击→Problems。  
  
Problems 页面按“修复动作”分组，而不是技术错误码：  
  
Configure storage  
“5 张照片等待配置存储”  
→ Configure  
  
Reconnect storage  
“GitHub storage needs reconnection”  
→ Reconnect  
  
Protection incomplete  
“2 项尚未达到当前保护策略”  
→ View details / Retry  
  
Encryption access issue  
→ Restore/Fix（T09 冻结具体恢复模型）  
  
Provider unavailable  
若系统仍自动 retry，默认不要求用户立即处理；只有进入 Needs Attention 才进入 Problems。  
  
十七、Photo Protection Detail  
  
从问题项或照片上下文进入，不要求 V1 做完整照片详情页。  
  
展示：  
Protection Mark：  
Important / Private 等。  
  
Current state：  
Protected / Protecting / Needs attention / Partially protected  
  
Policy facts：  
例如：  
“需要 1 个已验证远端副本”  
“上传前本地加密：Required”  
  
Actual copies：  
Local source  
GitHub Storage #1  
状态：Verified / Uploading / Unverified / Unavailable  
  
Last verified：  
时间事实。  
  
动作按状态出现：  
Retry  
Configure storage  
Reconnect  
Change protection  
不提供无关编辑功能。  
  
十八、Storage Instance Detail  
  
展示：  
Name  
Plugin  
Status  
Capabilities  
Configuration 摘要（不展示 secret）  
Last successful test  
Problems  
Test again  
Edit configuration  
Disable/Remove  
  
Remove StorageInstance：  
V1 必须二次确认，因为可能让某些 PhotoAsset 从 Protected 变为 Degraded/Needs Attention。  
确认前 Core 先计算影响：  
“Removing this storage will cause 12 protected items to no longer meet their
current policy.”  
不得承诺远端数据会同步删除。  
“Remove connection”与“Delete remote data”必须是两个概念。  
V1 默认 Remove 只移除本地连接/实例，不主动删除远端内容。  
  
十九、文案状态映射  
  
Domain → 用户文案建议：  
  
PENDING → Protecting / 正在保护  
PROTECTED → Protected / 已按当前策略保护  
DEGRADED → Protection incomplete / 保护不完整  
NEEDS_ATTENTION → Needs attention / 需要处理  
UNASSIGNED → 不显示技术状态  
  
禁止用户文案：  
Safe forever  
100% secure  
Never lose  
Military-grade  
Absolutely private  
Backup complete（除非精确定义上下文）  
  
Private 说明优先：  
“上传前在本地加密”  
而不是：  
“最高隐私等级”。  
  
二十、Loading / Success / Error 统一原则  
  
Loading：  
能局部加载绝不全屏阻断。  
防重复提交。  
  
Success：  
默认使用轻量 inline/toast/状态变化。  
不为每次打标弹“成功”对话框。  
  
Error：  
必须告诉用户：  
1\. 哪一步没完成。  
2\. 用户是否需要做事。  
3\. 可执行动作是什么。  
禁止只显示 Error Code。  
  
后台自动重试中的临时错误：  
不频繁打扰用户。  
只有需要人工动作才进入 Problems。  
  
二十一、关键导航闭环  
  
Flow 1｜首次使用  
Launch  
→ Home permission state  
→ Allow  
→ Home photos  
完成。  
  
Flow 2｜打 Important 且已有 Storage  
Home  
→ Select  
→ Protection Picker  
→ Important  
→ Home / Protecting  
→ Protected  
完成。  
  
Flow 3｜打 Private 且无 Storage  
Home  
→ Select  
→ Picker  
→ Private  
→ Storage Required  
→ Configure  
→ Plugin Store  
→ Plugin Detail  
→ Install  
→ Set Up  
→ Test  
→ Ready  
→ Continue  
→ Home / Encrypting+Protecting  
→ Verified  
→ Protected  
完成。  
无需重新选照片。  
  
Flow 4｜稍后配置  
Picker→Private→Storage Required→Not Now→Home Needs Attention  
→ Problems→Configure Storage  
→ 完成设置→自动 Re-evaluate  
→ Protected。  
  
Flow 5｜Credential 失效  
Home Problem Indicator  
→ Problems  
→ Reconnect Storage  
→ Test  
→ Ready  
→ 自动 Re-evaluate  
→ Home 状态恢复。  
  
二十二、最小页面数量验证  
一级：Home 1 个。  
必要二级：  
Storage Center  
Plugin Store  
Plugin Detail  
Plugin Setup  
Problems  
Protection Detail / Storage Detail 可用共享 Detail 模式实现。  
  
Protection Picker / Storage Required / Confirmations 为临时层。  
  
没有：  
Albums  
Search  
People  
Memories  
Editor  
Account  
Cloud Dashboard  
Social  
完整 File Manager  
  
符合最小 App。  
  
二十三、开发 AI 禁止自行决定事项  
\- 不新增底部 Tab。  
\- 不新增账号。  
\- 不新增首次教程轮播。  
\- 不把插件配置放首次启动。  
\- 不把 Storage 目标放 Protection Picker。  
\- 不把 Important/Private 做成单轴等级。  
\- 不自动删除远端副本。  
\- 不在 Private 加密失败时明文上传。  
\- 不把 Upload Success 显示为 Protected。  
\- 不新增照片编辑/搜索/相册功能。  
\- 不用 GitHub 专有概念污染 Core 页面。  
  
二十四、验证  
逐一映射 T04 用户可见状态：  
权限 unknown/partial/denied/empty/loading/error → Home 有对应 UI。  
Selection → 有明确 UI。  
Protection Picker → 有明确 UI。  
缺 Storage → Prompt。  
Marketplace/install/config/test → 各有 UI。  
Pending/Needs Attention/Degraded → Home/Problems/Detail 有 UI。  
Credential 失效 → Reconnect。  
Partial success → 每 PhotoAsset 独立状态。  
全部覆盖。  
  
“无猜测原型测试”：  
仅按本 T05，可完成从首次启动→授权→照片流→多选→Private→缺 Storage→安装 GitHub
Plugin→配置→测试→自动恢复→Protecting→Protected 的完整低保真原型，无需新增产品决策。  
  
二十五、对后续任务  
T06 必须给 Plugin Detail/Data Access/Capabilities/Configuration Schema 提供结构化数据。  
T07 必须支持 selected intent 落地、ResumeContext、Problem 聚合所需查询。  
T08 必须支持 UI 的 Protecting/Retry/Verified 实时状态来源。  
T09 必须确认插件 Data Access 文案可由权限模型真实支撑。  
T10 Acceptance Tests 必须逐 Flow 做 UI+Domain 双重验收。  
  
闭环结论：  
V1 UI 已冻结为“一个照片 Home + 按需展开的保护与插件配置流程”。所有核心状态均有明确呈现、动作和返回路径。低成本 AI
不需要自行设计产品导航和核心交互。  
  
T05：PASS。


```
