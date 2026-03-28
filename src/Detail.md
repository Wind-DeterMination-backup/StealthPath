# Detail

## 这一层是什么
`src/` 是源码根层。

## 这一层具体干了什么
- 采用标准 Gradle 约定，只启用 `main` 源集。
- 没有 `test/`、`androidTest/`、`generated/` 等自定义源码层。

## 与其他层级的关系
- 上游接受根目录构建脚本的约定。
- 下游分流到 `src/main/java/` 和 `src/main/resources/`。
