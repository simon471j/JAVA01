# 第一章节
## 构建工具
- 以前常用的是添加jar包
- 现在创建Maven工程 在里面添加依赖会自动下载jar包
### 好处
- 自动帮程序员甄别和下载第三方库
- 完成整个项目编译 （调用javac.exe）
- 完成整个项目单元测试流程 （调用JUnit工具）
- 完成项目打包 （jar/war等格式 调用jar.exe）

### 当前主要的Java构建工具
- Maven
- Gradle
- Ivy
- Buildr
- Ant

## Maven
### 开发流程
- 新建Maven项目
- 再中央仓库查找第三方jar的依赖文本
- 拷贝依赖至项目的pom.xml
- 执行maven build 编译/构建整个项目
- Maven是一个软件
- Maven也有一个中心仓库
- Maven是一个构建工具 自动下载中心仓库的jar文件 存在本地进行管理，编译、测试、运行、打包发布Java项目

### 生命周期
- 清理
- 编译
- 测试
- 打包
- 安装
- 部署