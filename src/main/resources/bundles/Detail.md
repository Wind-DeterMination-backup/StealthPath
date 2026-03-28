# Detail

## 这一层是什么
`src/main/resources/bundles/` 是整个模组的文案与本地化层。

## 这一层具体干了什么
- 提供热键名称、设置标题、设置描述、提示 toast、F8 调试日志、OverlayUI 标题等全部可见文本。
- 让 `StealthPathMod`、`GithubUpdateCheck`、设置组件统一通过 key 取文案，而不是写死字符串。

## 内容构成
- 当前共有 35 个 bundle 文件。
- `bundle.properties` 是默认语言包，键数 227。
- `bundle_zh_CN.properties` 也是完整语言包，键数 227。
- 其余语言包多为旧版本快照：
- 大多数语言包只有 55 个键，缺失 172 个新键。
- 西语、法语、俄语约 94 个键，仍缺 133 个。
- 繁中约 93 个键，仍缺 134 个。

## 实现方式
- 所有键采用 Mindustry/Arc 习惯的 `setting.*`、`sp.*`、`keybind.*` 命名。
- 这一层不仅覆盖 UI 文本，还覆盖 F8 控制台日志模板，因此它和行为逻辑耦合较深。
- 日志 key 如 `sp.log.plan.search`、`sp.log.rts.send` 说明作者把调试输出也做了本地化。

## 与其他层级的关系
- 被 `StealthPathMod.java` 和 `GithubUpdateCheck.java` 广泛读取。
- 与 `README_*.md` 不同，bundle 是运行时文案，README 是仓库说明；两套翻译系统相互独立。

## 当前状态
- 默认包和简中包是当前最完整的事实来源。
- 其他语言包明显滞后，新设置项和更新模块相关键值大面积缺失。
