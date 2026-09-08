# 03｜页面研究

Things 的“页面”大多不是独立数据容器，而是对同一实体集合的不同 Projection。页面研究必须同时描述 Route、Query、组件组合、UI State、跨视图 mutation 和 A 级证据缺口。

导航层可概括为：

* N1：Main Lists / System Lists / Area / Project / Special Lists；
* N2：Todo 原地展开 Presentation，不是普通 Detail Route；
* N3：When / Deadline / Tags / Move / Repeat 等 Picker / Modal；
* N4：Quick Find 全局 Overlay；
* N5：Selection / Drag 等 Interaction Mode；
* N6：Inline Editing / Expanded 等局部状态。
