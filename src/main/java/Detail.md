# Detail

## 这一层是什么
`src/main/java/` 是 Java 源码根目录。

## 这一层具体干了什么
- 当前只包含一个包：`stealthpath`
- 说明整个模组没有做多包分层，而是把核心逻辑集中在一个包内部，再靠文件拆分来控制复杂度

## 与其他层级的关系
- 下游 `stealthpath/` 是真正的实现层。
- 编译后会映射到 `bin/main/stealthpath/`、`build/classes/...` 和最终归档里的 `stealthpath/*.class`。
