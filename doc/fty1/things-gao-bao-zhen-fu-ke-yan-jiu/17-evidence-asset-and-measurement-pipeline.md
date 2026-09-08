# 17｜Evidence Asset & Measurement Pipeline

本页解决第 11 章指出的“真机证据资产与测量管线缺失”。

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

每一条证据记录：`evidenceId / sourceFile / device / appVersion / orientation / encodedSize / fps / timeRange / page / component / state / evidenceGrade / notes / hash`。

不允许使用“之前有个录屏里大概看到过”作为可执行依据。

## 测量产物

每次 C 级测量必须记录：原图/帧、crop 坐标、坐标系、测量值、单位、采样方法、置信度、受压缩影响说明。

示例：

```yaml
measurement_id: C-H02-checkbox-001
evidence_id: A-recording-202609xx-01
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

每个组件定义 Stable ID 和 Named Anchor。Inspector 输出统一 JSON：`id / x / y / width / height / baseline? / visible / state`。机器报告直接指出偏差，例如 `title.baseline actual=143 expected=142 delta=+1vp`。

## Diff 规则

采用分层 Gate：Structure、Geometry、Typography、Color、Screenshot、Interaction。iPhone 与 HarmonyOS 不做未经归一化的整屏像素硬比较；系统栏/设备比例/字体抗锯齿要通过内容 crop 与 mask 处理。

## Golden 管理

Golden 只能由强模型/人工验收流程创建或更新；3B Coder 永远没有 Golden 写权限。已验收 HarmonyOS Golden 用于回归，Things 真机截图作为目标证据，两者角色不同。

## 当前缺口

历史真机录屏尚未正式进入工程 Evidence Registry，因此精确 Token 继续标 `PENDING_EVIDENCE / CALIBRATION_REQUIRED`。这不阻止结构/Domain/交互合同先实现，但阻止宣称像素级 VERIFIED。
