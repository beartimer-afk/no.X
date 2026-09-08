# 统一数据中间层：讨论演进记录

> 这不是最终 PRD，而是一份像书一样持续生长的产品思想记录。重点不是只保存结论，而是保存问题如何被发现、方案为何改变、边界为什么被修正。

## 一、从照片开始：不同数据不该获得相同待遇

最初的问题是个人照片。家庭照片、Private 内容、普通生活记录、临时截图的价值和隐私完全不同。传统相册却倾向于用同一种存储方式处理全部数据。

第一个重要判断因此出现：

> **不同数据不应该获得完全相同的存储待遇。**

用户应该表达 Important / Private / Standard 等意图，由系统决定保存位置、副本数、是否加密，以及什么时候才真正完成保护。

进一步形成：

> **Upload Success ≠ Verified ≠ Protected。**

只有真实状态满足 Protection Policy 才能称为 Protected。

## 二、Asset ≠ Copy：从同步器走向 Personal Data Control Plane

现实中数据已经散落在百度网盘、NAS、硬盘、S3、OSS、WebDAV 等位置。真正的问题逐渐变成：我到底有什么，它们分别在哪里？

同一文件在四个 Storage 上并不是四个逻辑文件，而是一个 Asset 的四个 Copy。

因此：

> **Asset ≠ Copy。**

对于历史数据，Inventory 应先于 Sync：

**Connect → Scan → Index → Deduplicate → See Everything → Decide → Protect → Verify**

用户最终管理的是数据意图，而不是物理位置。

## 三、Storage Abstraction：统一数据层的核心

Provider 极度碎片化，每家都有不同 API、Auth、Credential、Path/Object 模型、限流、校验和错误模型。

如果每个 App、每个平台重新接一次，开发成本无法承受。因此底层必须形成统一 Storage Plugin Protocol。

上层只关心统一能力，例如 list / stat / read / write / delete / verify；Provider 差异全部留在插件层。

## 四、跨平台讨论改变了技术选型

当目标扩展到 iOS、Android、macOS、Windows、Linux、HarmonyOS 后，我们比较了 Flutter、KMP/Compose、React Native、Tauri/Rust 等方案。

真正改变选择的是一个约束：

> **Storage Provider 对接成本很高，插件绝不能每个平台重写一次。**

百度、Google Drive、OneDrive 等 Provider 未来可能涉及 OAuth、Token Refresh、分页、限流、断点续传、校验和错误码。这种成本不能乘以六个平台。

因此冻结原则：

> **插件不能属于 Flutter、Kotlin、Swift 或某个 UI 框架。插件必须属于独立跨平台运行时。**

当前优先验证的长期核心是：

* Rust Core
* WASM Plugin Runtime
* Versioned Plugin API / SDK
* Provider Plugins

UI 是可替换层。

## 五、Rust Core + WASM Plugin：一次开发，多平台消费

目标结构：

iOS / Android / macOS / Windows / Linux / HarmonyOS → UI / Platform Adapter → Rust Core → Versioned Plugin API → WASM Plugin Runtime → S3 / OSS / NAS / WebDAV / 百度 / Google / ...

理想状态是同一个 provider.wasm 多平台运行；如果某平台必须重新 target build，也可接受，但 **Provider 业务源码必须只有一份**。

平台代码只能处理文件选择、Sandbox、Keychain/Keystore、后台任务、系统 Auth Session 等平台问题，禁止出现 Provider × Platform 的重复业务实现。

## 六、Sync Engine 与 Provider Plugin 必须分离

Provider 插件不能决定“Important 应该保存几份”。

Core 的 Sync/Policy Engine 负责 Rule、Queue、Retry、Integrity、Desired State、Encryption requirement 和 Completion condition。

Provider Plugin 只负责 Provider-specific 的读写能力。

例如“Private → NAS + OSS，两份成功并验证才算完成”属于 Core；NAS/OSS 插件只是执行。

## 七、OAuth 不能成为新的重复劳动

讨论百度网盘时发现，Provider 的高成本不仅是文件 API，还有 OAuth callback、client secret、authorization code、access token、refresh token。

因此认证也必须共享，不能出现“百度 OAuth for iOS / Android / Windows”。

调研 Google Drive、OneDrive、Dropbox、Box、百度、阿里 PDS、115、S3、WebDAV、Nextcloud、坚果云、NAS 后，认证大体可归为：

### OAuthPKCE

支持 Native OAuth + PKCE 的 Provider。

### OAuthBroker

需要保护 client\_secret、固定 HTTPS callback 或服务端 code/token exchange 的 Provider。

### CredentialInput

S3 Access Key、WebDAV App Password、NAS 账号等。

### ProviderSpecific

特殊厂商流程的兜底扩展点。

目标是：

> **Provider Complexity + Platform Complexity，而不是 Provider Complexity × Platform Complexity。**

## 八、谁负责弹授权 UI？

WASM Plugin 不绘制登录 UI。

Provider Plugin / AuthSpec 描述认证规则；Rust Auth Coordinator 负责 state、PKCE、session、credential lifecycle；Platform Host 只提供通用系统 primitive：

* open\_external\_browser
* open\_auth\_session
* receive\_deep\_link
* show\_device\_code
* show\_qr\_code
* request\_credentials

平台可以各写一次薄桥接，但不能每个 Provider 再写一遍。

## 九、Auth Broker 可以存在，但不能成为数据服务器

某些 OAuth 流程确实需要服务器接 callback 或保护 client\_secret，因此允许一个极薄 Auth Broker。

它只处理必要认证协议。文件路径仍然应该是：

**用户设备 → Storage Provider**

而不是：

**用户设备 → 我们服务器 → Storage Provider**

个人产品的服务器默认不接触原始文件、Private 明文、主加密密钥和本地 Library。

Token 也优先探索完成 OAuth 后一次性交付设备、不长期保存在服务器的模式；强制服务端参与的 Provider 再单独记录限制。

## 十、PoC：不要用漂亮架构代替真实证据

第一个最小验证选择 GitHub Provider：

本地测试文件 → Host → Shared Runtime → 同一个 GitHub Storage Plugin → GitHub Repository。

验收重点不是 UI，而是 Provider 业务实现是否真正只存在一份。

随后增加四个关键 PoC：

### A — Provider 跨平台复用

同一个 WASM Provider 多平台真实上传。

### B — OAuth 跨平台复用

同一个 Rust Auth Coordinator + AuthSpec 至少两个平台完成浏览器授权、callback、token 和真实 API。

### C — 1GB Streaming

证明大文件不整体塞进 WASM 内存，验证 chunk/stream、Host-WASM 数据拷贝和峰值内存。

### D — 中断恢复

验证后台、进程终止、重启后能够识别未完成任务，并判断续传或安全重试路径。

最终必须给 PASS/PARTIAL/FAIL，以及 GO / GO WITH CHANGES / NO-GO。

## 十一、一次有价值的岔路：设备之间 Local Sync

一度设想 Mac、Windows、Android、HarmonyOS、iPhone 都有 Sync 目录，任意设备放入文件后其他设备自动感知。

随后纠正了边界：

> **Local Device Sync 只是一个插件/Provider，不能反过来定义整个 Core。**

本地设备、NAS、S3、百度、WebDAV 在架构上应该平级。

直接设备同步又会引出 Discovery、Notification、P2P、Relay 等复杂度。当前是个人使用，而且本地已有 NAS，因此没有必要提前承担这些成本。需要局域网共享时，NAS/SMB/WebDAV/SFTP 可以作为 Local Network Storage Plugin。

这个过程留下一个重要原则：

> **不要因为某个使用场景方便，就让这个场景侵入底层抽象。**

## 十二、从个人工具突然出现的第二条路线：Storage Integration SaaS

讨论 OAuth callback、服务器配置和 SDK 后，出现了新的产品机会。

个人开发者接 Google Drive、OneDrive、Dropbox、百度、阿里、115、S3 等平台时，需要面对 Developer App、callback、HTTPS server、client\_secret、code exchange、token lifecycle、Provider SDK、分页、大文件、限流和错误码。

这些基础设施可能比业务本身还烦。

因此统一数据层的能力可以对外产品化：

> **Storage Integration Platform / Unified Storage SDK**

而不只是“OAuth SaaS”。

## 十三、SaaS 第一层：Managed Auth / Callback Hosting

开发者希望做到：

storage.connect("baidu") storage.connect("google-drive") storage.connect("onedrive")

平台负责 OAuth callback hosting、client secret protection、code/token exchange、token refresh、credential encryption 和 Provider-specific auth quirks。

非常明确的购买理由是：

> **“我不想为了接一个网盘，再搭一套 OAuth 后台。”**

## 十四、SaaS 第二层：Unified Storage SDK

更大的价值是把 Storage API 统一：

storage.list("/") storage.upload(...) storage.download(...) storage.delete(...) storage.stat(...)

底层不管百度、Google Drive、OneDrive、Dropbox、S3 或 WebDAV，都由 Provider Plugin 消化差异。

于是为个人 App 构建的 Provider 插件资产，同时可以成为 SaaS 的核心资产。

未来可以探索：

* Cloud Mode
* Embedded Mode
* Self-hosted Mode

## 十五、Token 托管也可以成为产品能力

未来可以有不同 Credential 模式：

### Managed

Token 加密保存在平台服务器，体验最简单。

### Bring Your Own Vault

客户控制自己的 Secret/Vault。

### Ephemeral / Client-held

服务端只完成 OAuth，Token 一次性交付客户端，不长期保存。

这让开发便利和“用户拥有数据”的哲学可以同时存在，但必须明确不同 Trust Boundary。

## 十六、逐渐清晰的双重资产

### 用户侧资产

Rust Core、Sync/Policy/Verification、Encryption、Personal Library、Credential Manager、Plugin Runtime。

### 开发者平台资产

Plugin Specification、Provider Plugins、Auth Specification、Auth Coordinator、Auth Broker、Unified Storage SDK、Provider compatibility knowledge。

两条路线共享最昂贵的 Provider 对接资产。

## 十七、Local-only 原则的升级

早期说“Core 永久 Local-only”。加入 Auth Broker / SaaS 后，更准确的表达应该是：

> **个人产品的数据面默认 Local / Direct-to-Provider；任何服务器能力必须按明确 Trust Domain 隔离。**

个人产品默认不让服务器接触原始文件、Private 明文、主加密密钥、本地 Library/Index。

如果未来开发者主动选择 Managed SaaS，那是另一种明确的信任模型，不能偷偷改变个人产品的隐私承诺。

## 十八、长期哲学：Library 永久，App 可以替换

Photos、Reader、Music、Files 都只是 Experience。

传统模式常常是：

> **App = 数据容器。**

我们希望反过来：

> **Library 是永久的，App 是临时的。**

文件仍然在用户自己的 Storage；应用只是数据的视图和操作壳。

因此长期开放 Plugin API、Metadata Schema、Encryption Format、CLI/SDK 等仍然有意义。

甚至可以形成承诺：

> **Even we are replaceable.**

## 十九、当前形成的关键原则

1. 数据价值不同，Storage Policy 应该不同。
2. 用户表达 Intent，不管理 Transfer Task。
3. Asset 与 Copy 分离。
4. Inventory 在 Sync 之前。
5. Protected 是 Desired State 满足后的计算结果。
6. Private 必须在客户端/Core 加密。
7. Storage Provider 是插件，不进入平台 UI 业务代码。
8. Provider 业务源码只能维护一份。
9. UI/平台适配是可替换层。
10. Rust Core + WASM 是当前优先验证路线；可以换 Runtime/ABI，但不能轻易放弃 Provider 单一实现原则。
11. Sync/Policy Engine 与 Provider Plugin 分离。
12. Auth 也必须抽象。
13. Platform Host 只提供通用系统 primitive。
14. Auth Broker 可以存在，但个人数据面不经过它。
15. Local Device Sync 只是插件场景之一。
16. NAS 是当前个人局域网共享的简单解法。
17. Provider 插件资产可以同时服务个人 App 与开发者 SaaS。
18. Library 长期存在，App 可以替换。
19. 开放协议符合“用户拥有数据”的价值主张。
20. Vision 可以很大，但每一步必须用真实 PoC 验证。

## 二十、仍然值得继续讨论的问题

* WASM Runtime 在 iOS / Android / HarmonyOS 的工程表现。
* 1GB 甚至几十 GB 文件的 Streaming ABI。
* Host 与 WASM 如何减少数据复制。
* Plugin Permission / Capability Model。
* Plugin ABI 长期版本兼容。
* 是否允许 Native fallback。
* AuthSpec 属于 Plugin Manifest 还是独立 Registry。
* Refresh Token 最终由谁持有。
* Managed / Embedded / Self-hosted 是否都值得做。
* SaaS MVP 最先支持哪几个 Provider。
* Unified Storage SDK 如何处理 Provider-specific capability。
* Provider 接入是否能形成足够高的商业壁垒。
* Consumer 产品与 Developer SaaS 是两个产品还是一个 Core 的两种发行形态。
* Personal Library 的开放协议、Asset Identity、Metadata、Recovery 和多设备状态。

这些问题不需要现在写死，应该随着真实实现继续追加。

## 二十一、这一阶段的一句话总结

最开始，我们想解决：

> **“如何让照片安全地放到用户自己的 Storage。”**

后来问题变成：

> **“如何让任何 App 都不用关心数据到底在哪个 Storage。”**

再后来又发现：

> **“如果 Provider 对接本身这么贵，为什么只为自己的 App 做？能不能把认证、回调、Token 生命周期和统一 Storage SDK 也提供给其他开发者？”**

于是统一数据中间层逐渐形成两个互相增强的方向：

**Personal Storage Layer** —— 用户拥有自己的数据和 Storage。

**Storage Integration Platform** —— 开发者不必重复承担各家 Storage 的接入成本。

贯穿始终的核心资产不是某个照片页面，也不是某个 UI 框架，而是：

> **Storage Abstraction + Provider Plugins + Auth Abstraction + Policy/Verification + Open Protocol。**

这也是为什么讨论过程本身值得长期保存：最终产品决策会变化，但问题如何被发现、抽象如何被修正、哪些边界为什么被坚持，这些推理本身才是长期价值。
