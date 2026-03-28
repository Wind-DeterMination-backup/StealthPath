# Detail

## 这一层是什么
`dist/` 是仓库内对外发布层，是最接近“发给玩家”的目录。

## 这一层具体干了什么
- 存放桌面 jar/zip
- 存放安卓 jar
- 作为 GitHub Actions Release 上传源目录

## 当前文件形态
- `stealth-path.jar`、`stealth-path.zip`：当前主命名方案下的桌面/合并归档，大小一致，表示 zip 与 jar 内容本质相同，只是扩展名服务于不同使用习惯。
- `stealth-path-android.jar`：包含 `classes.dex` 的安卓包。
- `StealthPath.jar`、`StealthPath.zip`：大写项目名的旧命名残留，体积略有不同，明显不是当前脚本的标准输出名。

## 实现方式
- 由 `copyMergedJarToDist`、`copyMergedZipToDist`、`copyAndroidJarToDist` 这类任务复制进来。
- GitHub 工作流只认 `dist/*.jar` 与 `dist/*.zip`，所以这个目录是 CI 与人工发布的交汇点。

## 与其他层级的关系
- 上游是 `build/libs/` 和 `build/d8/`。
- 下游是 GitHub Release、玩家手动安装、以及上级 `构建/stealth-path/` 归档目录。

## 当前状态
- 目录里同时存在新旧命名两套产物，说明这里并非“只保留最新一次标准构建结果”的纯净目录，而是带有一定历史残留。
