# Detail

## 这一层是什么
`gradle/` 是 Gradle Wrapper 配套层。

## 这一层具体干了什么
- 存放 wrapper jar 与其配置。
- 让仓库在没有预装指定 Gradle 版本的机器上也能稳定执行构建。

## 与其他层级的关系
- 被根目录 `gradlew` 与 `gradlew.bat` 调用。
- 不理解项目业务逻辑，只负责启动正确版本的 Gradle。
