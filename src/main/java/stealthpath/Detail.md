# Detail

## 这一层是什么
`src/main/java/stealthpath/` 是模组的核心实现包。项目的玩法逻辑、寻路算法、UI、更新检查基本都在这里。

## 这一层具体干了什么
- `StealthPathMod.java`：5775 行，绝对主控文件。负责事件注册、热键、设置、实时预览、自动模式、RTS 下发、OverlayUI、深水重规划、调试日志。
- `StealthPathPathTypes.java`：抽出寻路/渲染数据结构，如 `ThreatMap`、`Node`、`PathResult`、`ControlledCluster`。
- `StealthPathPathUtil.java`：把 tile 路径转成世界坐标点、压缩转折点、生成 RTS 去重哈希。
- `StealthPathMathUtil.java`：颜色解析、tile/world 坐标换算、边界和 clamp。
- `StealthPathSettingsWidgets.java`：自定义设置组件，封装标题、开关、滑条、文本框。
- `StealthPathUiUtil.java`：小型 UI 布局辅助，目前主要负责设置面板推荐宽度。
- `GithubUpdateCheck.java`：启动后检查 GitHub Release，支持选择 assets、镜像下载、自动安装并重启。

## 关键实现链路
- 输入层：`update()` 每帧读取 X/Y/N/M/K/L/J 等热键状态。
- 设置层：`ensureDefaults()` 注册大量 `Core.settings` 默认值，`registerSettings()` 把这些键映射到 UI。
- 威胁层：`buildThreatMap()` 先填可通行图、地板伤害、护盾禁区，再叠加炮塔与单位威胁。
- 寻路层：`findPath()` 会先跑一次搜索，再对“会淹死”的路径做最多 4 次阻断重试；核心搜索由 `findPathAStar()` 或 `findPathDfs()` 完成。
- 自动移动层：`issueRtsMoveAlongPath()` 把路径变成路点，`scheduleRtsCommand()` 做节流，`runRtsCommand()` 真正发命令。
- UI 层：`ensureOverlayWindowsAttached()` 会优先用 MindustryX OverlayUI，失败时回退到 HUD。

## 这一层的实现细节
- `buildThreatMap()` 会用 `threatClearanceWorld` 影响 `map.safeBias`，把“离危险边缘更远”编码进 0 伤害路径的偏好。
- `applyThreatsToRisk()` 按 tile 中心点判断是否落在威胁圆环内，支持最小射程和威胁外扩。
- `findPath()` 的深水逻辑不是简单“遇到深水就禁用”，而是先尝试路径，再收集致命液段并重跑搜索。
- `pathWouldDrown()` 用“淹没进度”累计模型，危险液体会增加进度，安全路段会缓慢恢复进度。
- A* 的最小伤害模式不是纯最短路：代价里叠加了边伤害、距离偏置、拐弯偏置、微小 tie-breaker。
- SafeOnly 模式在 `map.safeDist` 存在时，会偏向安全走廊中心。
- OverlayUI 采用反射访问 MindustryX，不直接编译依赖对方类，从而保持“有则集成、无则回退”的客户端兼容性。

## 与其他层级的关系
- 上游依赖 `src/main/resources/bundles/` 提供所有设置与提示文案。
- 上游依赖根目录 `mod.json` 指向 `stealthpath.StealthPathMod`。
- 下游编译为 `bin/main/stealthpath/` 和各种 jar/zip 中的类文件。

## 当前状态
- 包结构已经做过一次“从主类里抽工具类和数据类”的减重，但主文件依然极大，说明项目目前仍是“单主类驱动 + 小型配套类”的形态，而不是彻底的多模块架构。
