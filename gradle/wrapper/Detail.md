# Detail

## 这一层是什么
`gradle/wrapper/` 是 Wrapper 的最小闭环目录。

## 这一层具体干了什么
- `gradle-wrapper.jar`：启动器字节码。
- `gradle-wrapper.properties`：声明分发地址、缓存路径、网络超时和 URL 校验。

## 实现方式
- 当前固定分发为 `gradle-8.14.3-bin.zip`。
- `validateDistributionUrl=true` 说明仓库要求对 Wrapper 下载源进行校验。

## 与其他层级的关系
- 由根目录脚本触发。
- 决定整个仓库执行 `gradlew` 时究竟使用哪一代 Gradle。
