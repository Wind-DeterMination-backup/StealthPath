# Detail

## 这一层是什么
`.github/` 是仓库托管平台相关配置层，只服务于 GitHub，不参与游戏运行。

## 这一层具体干了什么
- 承载 Actions 工作流配置。
- 让“代码仓库状态”与“发布产物生成规则”发生连接。

## 实现方式
- 当前目录下只有 `workflows/` 子目录，没有 issue template、pull request template、dependabot 等其它平台配置。
- 说明这个仓库的 GitHub 自动化重点只有“构建并发 Release”，而不是协作流程模板化。

## 与其他层级的关系
- 依赖根目录的 `build.gradle`、`gradlew`、`dist/` 约定。
- 自身不产出源码，只调度构建与发布。
