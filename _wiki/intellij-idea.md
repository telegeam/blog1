---
layout: wiki
title: IntelliJ IDEA
cate1: Java
cate2: Tools
description: 最好用的 Java IDE
keywords: IDEA, Java
---

快捷键基本与 [Android Studio](https://telegeam.github.io/wiki/android-studio/) 一致，这里重点记录解决遇到过的问题。

## Q&A

### 如何让代码自动换行显示？

Preferences -> Editor -> General，勾选 Soft-wrap these files，后面的文件名匹配改为 `*`，表示所有文件都启用自动换行。

### 如何加大控制台日志缓冲区大小？

有时候日志量太大，需要的信息被冲出控制台缓冲区，这种时候可以加大缓冲区大小，让它多缓冲一些行数。

Preferences -> Editor -> General -> Console，勾选 Override console cycle buffer size，然后修改后面的数字大小，默认是 1024（单位 KB），比如我修改为了 102400。

### 解决导入 Eclipse Maven 工程后无法读取 .xml 文件的问题

IDEA 与 Eclipse 配置文件目录的方式不同，可以将文件夹标记为 Sources、Resources 和 tests 等，而 src/main/java 默认被标记为 Sources，src/main/resources 才默认被标记为 Resources，编译时自动复制。

这样放在 src/main/java 目录下的文件与子文件夹均为 Sources，只将编译生成的 .class 文件复制到编译目录，在 Eclipse Maven 工程里放在 src/main/java 文件夹里的 xml、props 和 properties 文件就不会被拷贝到编译文件夹，导致执行时找不到这些文件，报类似下面这样的错误：

```
org.springframework.beans.factory.BeanDefinitionStoreException: IOException parsing XML document from class path resource [spring-demo.xml]; nested exception is java.io.FileNotFoundException: class path resource [spring-demo.xml] cannot be opened because it does not exist
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:343)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:303)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:180)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:216)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:187)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:251)
	at org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(AbstractXmlApplicationContext.java:127)
	at org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(AbstractXmlApplicationContext.java:93)
	at org.springframework.context.support.AbstractRefreshableApplicationContext.refreshBeanFactory(AbstractRefreshableApplicationContext.java:129)
	at org.springframework.context.support.AbstractApplicationContext.obtainFreshBeanFactory(AbstractApplicationContext.java:540)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:454)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:139)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:83)
	at org.mazhuang.demo.protocol.db.DemoContext.init(DemoContext.java:22)
	at org.mazhuang.demo.protocol.DemoServer.start(DemoServer.java:40)
	at org.mazhuang.demo.DemoSrv.main(DemoSrv.java:17)
Caused by: java.io.FileNotFoundException: class path resource [spring-demo.xml] cannot be opened because it does not exist
	at org.springframework.core.io.ClassPathResource.getInputStream(ClassPathResource.java:158)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:329)
	... 15 more
```

**解决方案：**

可以通过在 pom.xml 文件里添加 resources 配置来指定将哪些文件作为 resources 包含：

```xml
<build>
    <resources>
        <resource>
            <directory>${basedir}/src/main/java</directory>
            <includes>
                <include>**/*.props</include>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

### 如何导出 jar 包

File -> Project Structure -> Artifacts -> Click green plus sign -> Jav -> From modules with dependencies

Build -> Build Artifacts

### 去掉 UTF-8 文件的 BOM

UTF-8 文件带 BOM 里会编译报错：

```
Error:(1, 1) error: illegalcharacter: '\ufeff'
```

解决方法：

右键项目名称 -> Remove BOM

### FreeMarker 模板实时生效

1. 在配置文件里关闭 FreeMarker 缓存，设置模板更新延迟时间为 0：

    ```yml
    spring:
        freemarker:
            cache: false
            settings:
                template_update_delay: 0
    ```

2. 修改 IDEA 设置，开启 Build project automatically

    ![](/images/wiki/intellij-idea-auto-build.jpeg)

3. 双击 shift，输入 registry，然后回车选中图中项：

    ![](/images/wiki/intellij-idea-registry.png)

4. 找到并勾选 compiler.automake.allow.when.app.running：

    ![](/images/wiki/intellij-idea-auto-make.jpeg)

5. 重新启动项目。

    修改模板后可能会有几秒延时，刷新两遍就好了。

### 编辑器面包屑配置

1. 面包屑的显示/隐藏，两种方法：

    - 菜单 View - Active Editor - Show Breadcrumbs
    - 右键行号区域 - Breadcrumbs - Top / Bottom / Don't Show

2. 面包屑的位置：

    右键行号区域 - Breadcrumbs - Top / Bottom

### 提示 Invalid classes root

几种可能的解决步骤，可以逐一尝试：

1. 右键工程 - Run Maven - install -U，右键工程 - Run Maven - Reimport
2. File - Project Structure - Libraries - 全选删除，Reimport
3. 重启 IDEA
4. 关闭 IDEA，将 .idea 目录删除，重新打开项目

### 自动生成 serialVersionUID

当一个类实现 Serializable 接口后，在 IDEA 里默认不会提示生成 serialVersionUID。

做如下配置之后可以实现提示：

1. Preferences - Editor - Inspections
2. 搜索 Serializable class without 'serialVersionUID'
3. 勾选该项并确定

![](/images/wiki/idea-serializable-settings.png)

提示效果：

![](/images/wiki/idea-serializable-inspection.png)

### 配置打开/新建工程的默认 JDK 版本

打开/新建工程默认会使用比较新的 JDK，比如 JDK 11 或者 14。而在我的实际场景里一般还是需要用 JDK 8，这样每次都要手动设置很不方便。

设置默认 JDK 的方法：

File > New Projects Settings > Structure For New Projects

### 忽略文件

Java 项目里有一些文件是不需要关注的，但它们默认会展示在 Project 视图，甚至会出现在你的搜索结果里，形成干扰。比如 dependency-reduced-pom.xml 和 target 文件夹等。

如何忽略掉它们呢？

Preferences > Editor > File Types > Ignore Files and Folders，将文件/文件夹名称加进去，英文分号分隔。

### 修改查找的结果显示数量

默认显示 100 个查找结果，"100+ matches in X files"，而且无法翻页，所以想将 100 这个限制调大。

版本 >= 2021.2 的：Settings > Advanced Settings > Maximum number of results to show in Find In Path/Show Usages preview

老版本：Help > Find Action...Registry > ide.usages.page.size，改成更大的值，比如 10000。

### 设置程序/测试用例运行的默认 VM Options

经常需要通过 VM Options 指定一些环境和配置相关的参数，每次有新的程序或者测试用例要执行时都要手动配置非常麻烦。

设置默认的 VM Options 方法：

Run > Edit Configurations > Templates

点选你经常用到的 Template，比如 Spring Boot、JUnit、Tomcat Server > Local，然后在 VM Options 里填上默认参数后保存。

![](/images/wiki/idea-default-vm-options.png)

### 无法 import 自己工程中类

File - Invalidate Caches

参考：https://zhuanlan.zhihu.com/p/341969427

### 隐藏 Coverage 结果

Run - Show Coverage Data

### 编译项目时报 java.lang.OutOfMemoryError: GC overhead limit exceeded

```
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

修改 IDEA 的 Custom VM Options 并没有用。

这是因为运行 IDEA App，和编译的程序，使用不同的 JVM 进程。

此时需要修改编译配置：

Preferences - Build, Execution, Deployment - Compiler - Share build process heap size

默认是 700，我修改成了 4096，单位 Mbytes。


参考 <https://intellij-support.jetbrains.com/hc/en-us/community/posts/206166909-java-lang-OutOfMemoryError-GC-overhead-limit-exceeded>

### 更新时提示错误

```
IDEA does not have write access to /Application/IntelliJ IDEA.app/Contents. Please run it by a privileged user to update.
```

原因：最近切换过 MacBook 的用户，IDEA 是在老用户下安装的。

解决方法：

将 IntelliJ IDEA.app 目录的权限修改为当前用户。

```
sudo chown -R <current_user>:<user_group> "/Application/IntelliJ IDEA.app"
```

## 参考

- [解决IntelliJ IDEA无法读取配置*.properties文件的问题](http://www.cnblogs.com/zqr99/p/7642712.html)
- [How to build jars from IntelliJ properly?](https://stackoverflow.com/questions/1082580/how-to-build-jars-from-intellij-properly)
- [Can I adjust the “100+ matches in X files” from Intellijs “find in path”](https://stackoverflow.com/questions/44803519/can-i-adjust-the-100-matches-in-x-files-from-intellijs-find-in-path)
