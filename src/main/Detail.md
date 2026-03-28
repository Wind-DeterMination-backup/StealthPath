# Detail

## 这一层是什么
`src/main/` 是模组运行时主源集。

## 这一层具体干了什么
- `java/`：Java 逻辑实现
- `resources/`：运行时资源、bundle、mod 元数据

## 实现方式
- 完全遵循 Java/Gradle 默认布局，所以构建脚本里可以直接使用 `sourceSets.main.output`。

## 与其他层级的关系
- 上游是 `src/`
- 下游的 `java/` 和 `resources/` 一起构成最终归档内容
