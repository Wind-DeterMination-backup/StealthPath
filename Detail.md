# Detail

## 这一层是什么
仓库根目录是整个 `StealthPath` 模组的装配层。这里同时放了四类东西：可维护源码入口、玩家文档、构建与发布配置、已经生成的说明/产物。

## 这一层具体干了什么
- 通过 `build.gradle` 定义 Java 8 构建、Mindustry 依赖解析、D8 转换、桌面/安卓合包、以及复制到 `dist/` 和上级 `构建/` 目录的流程。
- 通过 `mod.json` 定义模组元数据：名称、版本、主入口 `stealthpath.StealthPathMod`、最小游戏版本 `154`、`hidden: true` 客户端模组属性。
- 通过 `README.md`、`RELEASE_NOTES.md`、`stealthpath_*_dox.md`、`DOC.md` 提供玩家说明、更新日志、维护者架构文档和一份合并后的总文档。
- 通过 `INDEX_dox.md` 和一批 `dox__*.md` 维护“文件到说明文档”的映射；这些文档由 `tools/generate_dox.py` 自动生成。
- 通过 `gradlew`、`gradlew.bat`、`gradle/wrapper/` 固化 Gradle Wrapper。
- 通过 `.github/workflows/release.yml` 把 tag 发布自动化到 GitHub Actions。

## 根目录下的重要内容分组
- 源码入口：`src/`
- 构建配置：`build.gradle`、`settings.gradle`、`gradlew*`、`gradle/`
- 发布输出：`dist/`、`build/`、`bin/`
- 文档：`README*.md`、`RELEASE_NOTES.md`、`DOC.md`、`stealthpath_*_dox.md`、`dox__*.md`
- 自动化：`.github/`、`tools/`
- 维护指令：`AGENTS.md`

## 实现方式
- 这个仓库不是“多模块项目”，而是一个单模块 Gradle Java 项目，`settings.gradle` 只定义了 `rootProject.name = "stealth-path"`。
- `README.md` 既面向中文玩家，也面向英文玩家；其他 `README_*.md` 是独立维护的翻译副本，而不是从主 README 自动同步生成。
- `DOC.md` 是聚合文档，把核心文档串成一个入口；它不是源码真相，而是便于人读的汇编视图。
- `dox__*.md` 是脚本生成的轻量文件级说明，不代替真实的源码阅读。

## 与其他层级的关系
- 向下连接 `src/main/java/stealthpath/`，那里是真正的行为实现层。
- 向下连接 `src/main/resources/bundles/`，那里是 UI 文案和本地化层。
- 向右连接 `.github/workflows/`，构建脚本与发布工作流共同决定 release 行为。
- 向上游输出到 `build/`、`dist/`、`bin/`，这些目录都是从本层配置驱动生成的。

## 当前状态与可见问题
- 主 README、`build.gradle`、`mod.json` 当前版本一致，为 `6.0.0`。
- `README_ar.md`、`README_es.md`、`README_fr.md`、`README_ru.md` 仍写着 `2.0.7`，属于明显滞后的玩家文档。
- `DOC.md` 内联的主 README 内容仍停在 `4.2.0`，说明聚合文档没有随主说明同步刷新。
- `INDEX_dox.md` 仍把 `mod.json` 映射到 `src/main/resources/mod.json`，而当前权威元数据文件实际位于仓库根目录，说明自动文档索引存在历史漂移。
- 本次目录说明没有深入 `.git/`、`.gradle/`、`build/tmp/` 这类版本库/缓存/临时目录；这些层是工具私有数据，不适合作为长期维护文档树的一部分。
