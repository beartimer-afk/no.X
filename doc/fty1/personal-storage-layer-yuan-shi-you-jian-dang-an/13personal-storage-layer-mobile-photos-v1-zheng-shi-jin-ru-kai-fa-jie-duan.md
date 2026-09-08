# 13｜Personal Storage Layer / Mobile Photos V1 正式进入开发阶段。

**邮件主题：** 【正式开发启动】Personal Storage Layer — Mobile Photos V1｜完整开发冻结规格

**收到时间：** 2026-09-06T03:44:48Z

## 邮件正文原文

```
Personal Storage Layer / Mobile Photos V1 正式进入开发阶段。  
  
本邮件附件《Personal_Storage_Layer_Mobile_Photos_V1_Development_Spec.md》是本项目 V1
的正式开发冻结规格，也是交给开发 AI 的权威输入。  
  
执行要求：  
1\. 开发 AI 必须先完整阅读附件，再开始编码。  
2\. 不允许重新设计已冻结的核心产品逻辑；若实现与规格发生不可调和冲突，应报告冲突，不得自行修改产品定义。  
3\. 按附件 M1 → M9 的依赖顺序实施，并以 AT-01 → AT-35 全部通过作为 V1 Done 标准。  
4\. 产品保持平台中立；具体平台适配在实施阶段处理，不反向污染 Core Domain。  
5\. Core 永久 Local-only；我们的服务端只允许 Plugin Marketplace Catalog / 版本 /
发布撤回信息，不保存用户照片、保护策略、Storage Credential、Key、RemoteCopy 或任务历史。  
6\. V1 唯一真实 Reference Plugin 为 GitHub；必须先用 Mock Plugin 验证 Core 与 Provider 解耦。  
7\. Private 必须在进入 Plugin / Provider 前由 Core 完成真实客户端本地加密；加密失败绝不允许明文降级上传。  
8\. Upload Success != Verified != Protected。只有 ProtectionEvaluation 能产生
PROTECTED。  
9\. Source 删除、Protection 改标、Remove Storage connection 均不得隐式删除远端数据。  
10\. GitHub 的认证/API/文件与仓库限制/Rate Limit/服务条款属于动态事实，真正实现 GitHub Plugin
前必须按当时官方资料重新验证。  
11\. 密码算法、KDF、nonce、key wrapping、Recovery Kit 格式属于安全工程 Gate。不得由低成本 AI
自行发明，必须采用成熟方案并经过独立安全评审。  
12\. Recovery Kit / Recovery Key 必须由用户自己控制，不上传我们的服务端；如果用户丢失本地 Key
且没有恢复材料，产品必须如实提示 Private 密文可能永久无法恢复。  
  
附件已经汇总并收敛此前 T01–T10 的产品研究、Domain、Protection Model、状态机、UI/交互、插件协议、GitHub
Reference Plugin、持久化模型、执行/恢复语义、Local-only Trust Boundary、Recovery 约束、开发顺序和
Acceptance Tests。  
  
从本邮件开始，项目状态定义为：正式开发启动。  
  
开发执行者请以附件为准，不需要回看此前长邮件线程才能开始开发。


```
