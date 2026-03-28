# Detail

## 这一层是什么
`.github/workflows/` 是 GitHub Actions 工作流定义层。当前只有一个 `release.yml`，职责非常集中。

## 这一层具体干了什么
- 在手动触发或推送 `v*` tag 时执行发布任务。
- 在 Ubuntu 环境里准备 Java 17、Gradle 和 Android SDK。
- 运行 `clean deploy`，要求仓库生成 `dist/*.jar` 与 `dist/*.zip`。
- 在 tag 触发时，把 `dist/` 下的 jar/zip 挂到 GitHub Release。

## 实现方式
- 使用 `actions/checkout@v4` 拉代码。
- 使用 `actions/setup-java@v4` 配置 Temurin 17。
- 使用 `gradle/actions/setup-gradle@v4` 缓存并初始化 Gradle。
- 使用 `android-actions/setup-android@v3` 和 `sdkmanager` 安装 `build-tools;34.0.0`，为 D8 提供运行环境。
- 构建命令优先用本仓库的 `./gradlew`，不存在时才退回系统 `gradle`。

## 与其他层级的关系
- 上游来自根目录构建脚本。
- 下游目标是 `dist/` 目录中的发布包。
- 它不理解 `src/` 细节，只要求 Gradle 任务契约稳定。

## 当前状态
- 工作流明确依赖 `deploy` 任务，因此根目录构建脚本已经把“桌面+安卓合并包”的生成职责收口到一个统一入口。
