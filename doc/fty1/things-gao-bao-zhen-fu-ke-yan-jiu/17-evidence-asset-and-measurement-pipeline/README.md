# 17｜Evidence Asset & Measurement Pipeline

本页解决第 11 章指出的“真机证据资产与测量管线缺失”。

## 当前状态

第一批 **6 份用户原始 Things iPhone 录屏已经完成资产级登记**：统一 Evidence ID、SHA-256、分辨率、FPS、时长、文件大小和初步内容标签已生成。它们现在可以作为后续 frame/crop 级证据索引的稳定源资产。

注意：`A_SOURCE_ASSET` 只表示原始真机录屏本身可信，不代表其中任意一个行为已经完成帧级索引。具体视觉/交互结论仍需记录时间段、frame、crop 和测量过程。

## 目标目录

```
evidence/
  iphone/
    recordings/
    screenshots/
    crops/
  manifest/
    evidence.json
measurement/
  anchors/
  color-samples/
  geometry/
visual-test/
  actual/
  golden/
  diff/
  overlay/
  reports/
```

## Evidence Manifest

每一条证据记录：`evidenceId / sourceFile / device / appVersion / orientation / encodedSize / fps / duration / page / component / state / evidenceGrade / notes / sha256`。

不允许使用“之前有个录屏里大概看到过”作为可执行依据。

## 测量产物

每次 C 级测量必须记录：原图/帧、crop 坐标、坐标系、测量值、单位、采样方法、置信度、受压缩影响说明。

```yaml
measurement_id: C-H02-checkbox-001
evidence_id: A-REC-001
frame: 1032
encoded_size: 512x1108
bbox_epx: [18, 410, 21, 21]
result: checkbox_outer=21epx
confidence: C
calibration_status: PROVISIONAL
```

## 颜色规则

视频编码色只能作为 Seed；不得写成 Final Token。最终 Token 需要原始截图/更高可信源 + HarmonyOS Golden 校准。组件禁止 raw literal color。

## Geometry Contract

每个组件定义 Stable ID 和 Named Anchor。Inspector 输出统一 JSON：`id / x / y / width / height / baseline? / visible / state`。机器报告直接指出偏差。

## Diff 规则

采用分层 Gate：Structure、Geometry、Typography、Color、Screenshot、Interaction。iPhone 与 HarmonyOS 不做未经归一化的整屏像素硬比较；系统栏/设备比例/字体抗锯齿要通过内容 crop 与 mask 处理。

## Golden 管理

Golden 只能由强模型/人工验收流程创建或更新；3B Coder 永远没有 Golden 写权限。已验收 HarmonyOS Golden 用于回归，Things 真机截图作为目标证据，两者角色不同。

## 下一步

1. 对 6 份录屏建立时间段级 Semantic Index。
2. 把已知 H02/H03/H04/H07/H08 的测量值绑定到具体 Evidence ID + frame/crop。
3. 真正工程创建后，把 manifest 与 measurement JSON 纳入仓库，而不是只留在文档。
