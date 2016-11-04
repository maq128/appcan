# 自助离线打包

在试用 appcan 的过程中，深深感觉到离线打包的必要性。

当然，appcan IDE 本身有打包功能，官方提供的在线打包系统也很不错，管理功能都很直观、方便。但在日常开发工作中，开发者经常会遇到一些困难，比如：

- 使用了自定义插件的项目无法在 IDE 中调试，任何修改都要在线打包才能调试。
- 在线打包系统如果遇到故障或者升级，会影响日常开发工作，甚至影响新版发布。
- 有时在线打包的人数较多，排队要等较长的时间。

本项目提供了一个利用 Android Studio 原生开发工具进行离线打包的环境，适用于下面一些场景：

- 阅读应用引擎源代码，深入学习研究，甚至自定义应用引擎。
- 开发自定义插件。
- 离线打包。
- 模拟器调试、真机调试。当然这里的调试指的是原生级，而不是 H5 级。

**本项目仅供学习研究使用，商业应用的 app 项目强烈建议使用官方提供的工具进行打包发布。**

## 程序来源

应用引擎 https://github.com/AppCanOpenSource/appcan-android

第三方插件 https://github.com/android-plugin

## 开始使用

- 安装并配置 Android Studio，达到可用状态。
- 下载本项目全部文件，如果是压缩包则解压至某个目录。
- 启动 Android Studio 并打开本项目，即可。
- 主菜单 -> Build -> Build APK/Generate Signed APK...

其它关于如何使用 Android Studio 的操作，这里不再赘述。

## 关键目录介绍

| 目录 | 说明 |
| :------------ | :------------ |
| /app/libs | 原生程序库文件。 |
| /app/src/main | 所有源程序基本都在这里。 |
| /app/src/main/assets/widget | app 的 H5 资源。本项目中这里的内容是随便写的，不要太在意。 |
| /app/src/main/java | 应用引擎的 Java 源程序。 |
| /app/src/plugin-camera | 插件的源程序和原生资源。每引入一个插件，都要建一个类似的目录，名字不是严格要求的，只要跟相关的配置信息符合就好。 |

## 常规使用

#### 如果你要把它改成“自己的项目”

这个主要是 app id 和 package 的问题。前者是在 /app/build.gradle 文件中修改 `applicationId` 项，后者是修改 /app/src/main/AndroidManifest.xml 文件中的 `package` 属性。你也可以在所有文件中搜索 `buffet` 字样并进行适当的替换。

#### 当你要修改应用引擎

应用引擎的 Java 源程序都在 /app/src/main/java 目录下。/app/src/main/res 下面是原生资源文件，包括启动屏图片、应用图标等。

#### 当你要修改 H5 内容

H5 资源都在 /app/src/main/assets/widget 目录下面，这里的内容跟使用 appcan IDE 开发时 phone 目录下的内容是一样的。

#### 当你要增加一个插件

- 为插件创建一个目录，比如 /app/src/plugin-my，并在下面建两个子目录，java 用于放置源程序文件，res 用于放置资源文件。
- 修改 /app/build.gradle 文件，在适当位置增加下面的内容：
```
	sourceSets.main.java.srcDirs += 'src/plugin-my/java'
	sourceSets.main.res.srcDirs += 'src/plugin-my/res'
```
- 把插件源代码中的 plugin.xml 文件内容手工合并到 /app/src/main/res/xml/plugin.xml 中去。
- 把插件源代码中的 AndroidManifest.xml 文件内容要手工合并到 /app/src/main/AndroidManifest.xml 中去。
- 如果插件需要依赖原生程序库文件，要复制到 /app/libs 下面。

以上内容可以参考本项目中已有的 plugin-camera 来处理。
