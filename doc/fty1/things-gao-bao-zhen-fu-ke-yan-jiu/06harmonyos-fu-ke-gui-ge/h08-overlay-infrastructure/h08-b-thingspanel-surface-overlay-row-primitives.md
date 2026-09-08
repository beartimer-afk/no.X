# H08-B｜ThingsPanel Surface / Overlay Row Primitives

> HarmonyOS 26 / ArkUI｜正式开发规格｜可独立验收

H08-B 只实现 Picker / Action Menu 共用视觉原语，不含 When / Deadline / Tags / Move / Repeat 业务。

## 唯一组件族

`ThingsPanelSurface / PanelHeaderSlot / PanelSection / PanelRow / PanelIcon / PanelPrimaryText / PanelSecondaryText / PanelTrailing / PanelSelectionIndicator / PanelDivider / PanelScrollBody`。

后续 Picker 禁止各自写 surface、row padding、字体、pressed style。

## 证据边界

当前 A 级证据确认 Deadline 使用深色独立 Picker；精确 RGB、radius、shadow、rowHeight、typography 等仍需 Golden 校准。设计生成图不能作为 Token 事实源。

## Surface

只负责 background、clip、radius、shadow、width constraints、content padding、scroll body。Anchor / SafeRect / Back / Scrim 属于 H08-A。

## Token

新增 `overlaySurface / overlayPrimaryText / overlaySecondaryText / overlayIcon / overlayPressed / overlaySelected / overlayDivider / overlayDestructive / overlayAccent` 以及 overlay radius/shadow/row size/spacing/typography/icon tokens。业务 UI 禁止 raw color/size literals。

## PanelRow

结构为 optional leading + text column + trailing。Primary/Secondary 与 trailing 不 overlap；secondary 只增加 row height；selected/pressed/disabled/destructive 不改变 geometry。

PanelRow 只发 `onActivate(rowId)`，不能自行修改 Domain 或关闭 Overlay。

## 状态

selected 只代表当前 Picker selection，不是 Todo Completion 或 Batch Selection。H08-B 提供状态接口，但不强迫所有 selected row 使用相同蓝底/勾。Pressed 禁止系统 ripple；Destructive 只提供 visual，真正命令由具体 Picker 决定。

## Scroll / Section

长列表由 `PanelScrollBody` 内滚，Panel external rect 不变。Section 仅做分组；禁止系统 List 默认 divider。Search 只预留 HeaderSlot，不在本任务提前实现。

## Stable IDs / Geometry

Surface、Section、Row、leading、primary、secondary、trailing、selection、divider 全部稳定 ID。Geometry anchors 能输出 baseline / trailing offset 的具体误差。

## Fixtures

empty、one row、icon、primary+secondary、value/checkmark/disclosure trailing、selected、pressed、disabled、destructive、two sections、divider、long text、narrow、font scale、20/100 rows、dark deadline seed、mixed states。

## 3B Capsules

H08B-01 token declarations；02 Surface；03 text row；04 icon slot；05 secondary；06 trailing；07 states；08 section/divider；09 scroll；10 accessibility；11 mixed fixture；12 Geometry/Golden；13 performance。

## Pending

exact dark RGB、radius、shadow、row geometry、typography、icon size、divider usage、pressed/selected/destructive visuals、motion integration。

## 完成定义

Core FUNCTIONAL\_VERIFIED：Surface / Row / Section / Scroll / states / accessibility / IDs / event isolation / Geometry PASS。

Visual PROVISIONAL\_VERIFIED：内部 Golden 可锁回归；精确 Things 视觉等 A 级 crop 校准。
