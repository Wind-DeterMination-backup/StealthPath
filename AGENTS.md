# AGENTS.md - StealthPath 开发指南

本文件用于指导在 `StealthPath` 项目内进行代码审查、功能开发与发布验证。

## 1) 项目目标与边界

- 项目名：`stealth-path`
- 类型：Mindustry Java 客户端模组（非服务端插件）
- 目标游戏版本：`minGameVersion: 154`
- 主入口：`stealthpath.StealthPathMod`
- 元数据文件：根目录 `mod.json`

## 2) 模组文件结构

- `build.gradle`：构建、打包、复制产物（`deploy` 任务）
- `mod.json`：模组元信息（name/version/main/minGameVersion）
- `src/main/java/stealthpath/`
- `src/main/java/stealthpath/StealthPathMod.java`：主逻辑（事件、威胁图、寻路、RTS 下发、自动模式）
- `src/main/java/stealthpath/StealthPathPathTypes.java`：ThreatMap/PathResult/Node/Cluster 等数据类型
- `src/main/java/stealthpath/StealthPathPathUtil.java`：路径压缩、路径哈希、坐标点转换
- `src/main/java/stealthpath/StealthPathMathUtil.java`：坐标与通用数学工具
- `src/main/java/stealthpath/StealthPathSettingsWidgets.java`：设置项 UI 组件
- `src/main/java/stealthpath/StealthPathUiUtil.java`：UI 布局辅助
- `src/main/java/stealthpath/GithubUpdateCheck.java`：版本检查
- `src/main/resources/bundles/bundle.properties`：默认文案
- `src/main/resources/bundles/bundle_zh_CN.properties`：中文文案
- `dist/`：构建产物输出目录

## 3) 核心算法关注点

- 威胁计算：`buildThreatMap()`、`applyThreatsToRisk()`、`collectTurretThreats()`、`collectUnitThreats()`
- 路径规划：`findPath()`、`runPathSearch()`、`findPathAStar()`、`findPathDfs()`
- 深水/液体判定：`pathWouldDrown*()`、`collectDrownBlockingTiles*()`、RTS 路点推进逻辑
- RTS 下发：`issueRtsMoveAlongPath()`、`scheduleRtsCommand()`、`runRtsCommand()`

## 4) 代码约束

- Java 8 兼容（`sourceCompatibility/targetCompatibility = 1.8`）
- 不引入新的第三方依赖
- 优先做最小侵入修改，避免与当前行为无关的重构
- 修改设置项或提示文本时，必须同步更新中英文 bundles
- 不提交无关格式化

## 5) 构建与发布工作流

每次功能修改后至少执行：

1. `gradle classes`
2. `gradle deploy`

`deploy` 期望产物：

- `dist/stealth-path.jar`
- `dist/stealth-path.zip`

并复制到工作区归档目录：

- `../构建/stealth-path/stealth-path-<version>.jar`
- `../构建/stealth-path/stealth-path-<version>.zip`

## 6) 变更后验证清单

- 选中单位后，自动模式 M/N 可持续出路径并下发 RTS 指令
- `sp-rts-waypoint-reach-tiles` 生效，单位接近路点后能推进到后续路点
- 涉及深水的地图上，地面可溺水单位不会被错误推进到危险深水段
- 路径在“可接受风险”前提下不过度绕远，拐点数量合理

## 7) 提交与回复规范

- 只提交与当前任务相关的文件
- 最终说明至少包含：
- 改动文件列表
- 验证命令与结果
- 产物路径（`dist` 与 `../构建/stealth-path`）

命令操作请使用 PowerShell 7（`pwsh`）。
