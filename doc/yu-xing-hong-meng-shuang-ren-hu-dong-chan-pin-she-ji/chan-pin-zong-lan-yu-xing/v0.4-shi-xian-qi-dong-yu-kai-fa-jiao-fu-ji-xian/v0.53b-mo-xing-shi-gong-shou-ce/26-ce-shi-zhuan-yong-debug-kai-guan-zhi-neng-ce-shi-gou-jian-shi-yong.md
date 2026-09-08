# 26｜测试专用 Debug 开关：只能测试构建使用

## 目标

让弱模型能够稳定复现 Tense / Fake / Explosion 等状态，而不是反复等待随机时间。

## 固定 Debug 配置

只允许在 Debug 构建存在：

```
DEBUG_FORCE_EXPLOSION_AFTER_MS: number? = null
DEBUG_FORCE_FAKE_AFTER_MS: number? = null
DEBUG_FORCE_TENSION_RATIO: number? = null
DEBUG_DISABLE_SOUND: boolean = false
```

放在专门 Debug 配置文件，不得放 Settings UI。

## 用法

### 测 Explosion

设置：

```
DEBUG_FORCE_EXPLOSION_AFTER_MS = 5000
```

只改变测试 explodeAt；UI 不得显示这是测试。

### 测 Fake

```
DEBUG_FORCE_FAKE_AFTER_MS = 2500
```

仍使用 Fake 动效正式代码路径。

### 测 Tense

```
DEBUG_FORCE_TENSION_RATIO = 0.80
```

只强制视觉 tension ratio，不得触发真实 Explosion。

## Release 构建要求

* 所有 Debug 值必须编译为 null/false 或不包含；
* Release 不得存在隐藏手势打开 Debug；
* Release 不得存在连续点击 Logo 之类开发入口。

## 禁止

* 通过改正式常量 14–30 秒做测试；
* 测完忘记恢复；
* 将 Debug 开关暴露在用户 Settings；
* Debug 代码另写一套 Explosion 逻辑。

## 验收

分别用 Debug 强制 Fake/Tense/Explosion，证明使用与正式相同组件和状态路径。
