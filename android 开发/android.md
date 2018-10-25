# android

## 四大组件

Activity(活动)、Service（服务）、(Broadcast Receiver) 广播接收器、ContendProvider(内容提供器)

## 目录结构

1. .gradle 和 .idea

   这两个目录放置的都是Android Studio 自动生成的一些文件，我们无需关心，也不需要动手去编辑

2. .app项目中的代码资源内容几乎全部都放在在这个目录下面，我们后面的开发工作基本在这个目录下进行。

3. .build

   这个目录也无需过多的关心，它主要包含了一些编译时自动生成的文件

4. .gradle

   这个目录下包含了 gradle wrapper的配置文件，使用gradle wrapper的方式不需要提前将gradle 下载好，而是会自动根据本地缓存情况决定是否需要联网下载gradle

5. .gitignore

   这个文件是用来将制定的文件目录或者文件排除在版本控制之外的

6. build.gradle

   这是项目全局的gradle构建的脚步，通常这个文件中的内容是不需要修改的。

7. gradle.properties

   这个文件是全局的gradle配置文件，在这里配置的属性将会影响到项目中所有的gradle编译脚步

8. gradlew 和gradlew.bat

   这两个文件是用来在命令行界面中执行gradle命令的，其中gradlew是在linux或者Mac系统中使用的，gradlew.bat是在Windows系统中使用的

9. HelloWorld.iml

   iml文件是所有InteliJ IDEAA项目都会自动生成的一个文件，用于标识这是个InteliJ IDEA项目，我们不需要修改这个文件的任何内容

10. local.properties

    这个文件用于指定本机中Android SDK路径，通常内容都是自动生成的，我们并不需要修改，除非你本机的Android SDK位置发生了变化。

11. settings.gradle

    这个文件用于指定项目中所有引入的模块由于HelloWorld项目中就只用一个app模块，因此文件中也就只引入了app、这一模块。通常情况下模块的引如都是自动完成的，需要我们手动去修改这个文件的场景比较少

## app目录下的内容

1. build 

   这个目录和外层的build目录类似，主要也是包含了一些编译时自动生成的文件，不过它的内容会更多更杂，我们不需要关心

2. .libs

   如果你的项目中使用了第三方jar包，就需要把这些jar包都有放到libs目录下，放在这个目录下的jar包会被自动添加都构建路径里去

3. AndroidTest

   此处是用来编写Android Test 测试用例的，可以对项目进行一些自动化测试

4. java

   毫无疑问，java目录是放在我们所有java代码的地方。

5. res

   这个目录下的内容有点多，简单点说，就是项目中使用到的所有图片、布局、字符串等资源都要存在这个目录下

6. AndroidManifest.xml

   这是你整个Android项目的配置文件，你在程序中定义的所有四大组件都需要在这个文件里注册，另外还可以在这个文件中给应用程序添加权限声明

7. test

   此处是用来编写Unit Test测试用例，是对项目进行自动化测试的另一种方式

8. .gittifgnore

   这个文件用于将app模块内的指定的目录或文件排除在版本控制以外，作用和外层的.Gitignore文件类似

9. app.iml

   InteliJ IDEA 项目自动生成的文件，我们不需要关心或者修改这个文件的内容、

10. build.gradle

    这是app模块的gradle构建脚本，这个文件中会指定很多项目构建相关的配置

11. proguard-rules.pro

    这个文件用于指定项目代码的混淆规则，当代码开发完成后打包完成后打成安装包文件，如果不希望代码被人破解，通常会将代码进行混淆，从而让破解者难以阅读
