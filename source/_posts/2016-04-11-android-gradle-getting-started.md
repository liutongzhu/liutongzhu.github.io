title: Android Gradle 入门
tags:
  - Android
  - Gradle
categories:
  - Android
  - Gradle
toc: true
date: 2016-04-11 21:49:50
toc: true
---

 Gradle是一款非常优秀的构建系统工具,它的DSL基于Groovy实现,可以让你很方便的通过代码控制这些DSL来达到你构建的目的，其构建的大部分功能都是通过插件的方式来实现，所以非常灵活方便，如果内置插件不能满足你的需求你可以自定义自己的插件。

本章我们就介绍Gradle的入门，在介绍之前，我们先假定读者已经具备以下前提：

1. 了解并且会使用Java，精通最好
2. 会自己的独立的搭建Java开发环境
3. 最好会使用Linux操作系统，比如Ubuntu

为什么会有这样的假定呢，因为这本书是介绍Android Gradle开发构建的书，所以不会花章节去讲Java的基本知识，能希望读者会用Linux操作系统的原因，是因为作者该书的所有脚本、代码、IDE等都是基于Ubuntu完成的，当然比如涉及到Gradle安装还会介绍一下Window的安装步骤，但是不会太多涉及到Window的东西，这个关于本书一章里有提到，所以呢，还是希望读者们在阅读本书前已经掌握了这些知识，Linux操作系统非必须，因为涉及到的都是Linux很基本的东西，大家很容易就看得懂。

### 配置Gradle环境

安装之前确保已经安装配置好Java环境，要求JDK 6以上，并且在环境变量里配置了JAVA_HOME,查看java版本可以在终端输入如下命令：
```shell
java -version
```
我这里使用的是open jdk 1.7.0_79。
```shell
flysnow@flysnow:~$ java -version
java version "1.7.0_79"
OpenJDK Runtime Environment (IcedTea 2.5.6) (7u79-2.5.6-0ubuntu1.15.04.1)
OpenJDK 64-Bit Server VM (build 24.79-b02, mixed mode)
```
#### Linux下搭建Gradle构建环境

这里以Ubuntu 15.04发行版为例介绍如何在Linux搭建Gradle构建环境，其他注入CentOS大同小异，参考下就可以了。

我们这里以Gradle 2.5版本为准进行介绍，先到Gradle官网 http://gradle.org/下载好Gradle SDK，直接下载地址为 https://downloads.gradle.org/distributions/gradle-2.5-all.zip，我们下载的是all版本，也就是说，里面包含了Gradle SDK所有相关的内容，包括源代码、文档、示例等。下载之后进行解压，我们可以得到如何目录清单：

1. docs----API、DSL、指南等文档
2. getting-started.html----入门链接
3. init.d----gradle的初始化脚本目录
4. lib----相关库
5. LICENSE
6. media----一些icon资源
7. NOTICE
8. samples----示例
9. src----源文件

要运行Gradle，必须把GRADLE_HOME/bin目录添加到你的环境变量PATH的路径里才可以。在Linux下，如果你只想为你当前登陆的用户配置可以运行Gradle，那么可以编辑～/.bashrc文件添加以下内容：
```shell
#这里是作者的Gradle目录。要换成你自己的
GRADLE_HOME=/home/flysnow/frame/gradle
          
PATH=${PATH}:${GRADLE_HOME}/bin
exportGRADLE_HOME PATH
```
上面GRADLE_HOME是我的Gradle解压后的目录，这里要换成你自己的。以上添加后保存，然后在终端输入source ~/.bashrc回车执行让刚刚的配置生效。

如果你想让所有用户都可以使用gradle，那么你就需要在/etc/profile中添加以上内容，在这里添加后，对所有用户都生效，这种方式的添加，必须要重启电脑才可以。

好了，现在我们已经配置好了，要进行验证我们的配置是否正确，是否可以运行gradle，我们只需要打开终端，输入gradle -v命令查看即可，如果能正确显示gradle版本号、Groovy版本号、JVM等相关信息，那么说明你已经配置成功了，这里以我的为例：
```shell
$ gradle -v
          
------------------------------------------------------------
Gradle 2.5
------------------------------------------------------------
           
Build time:   2015-07-08 07:38:37 UTC
Build number: none
Revision:     093765bccd3ee722ed5310583e5ed140688a8c2b
           
Groovy:       2.3.10
Ant:          Apache Ant(TM) version 1.9.3 compiled on December 23 2013
JVM:          1.7.0_79 (Oracle Corporation 24.79-b02)
OS:           Linux 3.19.0-28-generic amd64
```

#### Window下搭建Gradle构建环境

Window下搭建Gradle环境和Linux非常相似，只不过方式不同。我们通过右击我的电脑，打开属性面板，然后找到环境变量配置项，添加GRADLE_HOME环境变量，然后把GRADLE_HOME\bin添加到PATH系统变量里保存即可。完成后打开CMD运行gradle -v来进行验证，整体和Linux差不多，这里就不在一一详述。

### Gradle版Hello World

环境搭建好了，那么我们就开始写一个Hello World版的Gradle脚本吧。

新建好一个目录，我这里是android-gradle-book-code，然后在该目录下创建一个名为build.gradle的文件，打开编辑该文件，输入以下内容：
```groovy
task hello{
    doLast{
        println'Hello World!'
    }
}
```
打开终端，然后移动到android-gradle-book-code下，使用gradle -q hello命令来执行构建脚本：
```shell
$ gradle -q hello
Hello World!
```
好了，如愿以偿的打印出来我们想要的结果，下面我们一步步分析结果产生的步骤和原因。build.gradle是Gradle默认的构建脚本文件，执行Gradle命令的时候，会默认加载当前目录下的build.gradle脚本文件，熟悉Ant的同学感觉和build.xml差不多，当然你也可以通过 -b 参数指定想要加载执行的文件。

这个构建脚本定义一个任务（Task），任务名字叫hello，并且给任务hello添加了一个动作，官方名字是Action，阅读Gradle源代码你会到处见到它，其实他就是一段Groovy语言实现的闭包，在这里我觉得叫业务代码逻辑或者回调实现更贴切一些，因为doLast就意味着在Task执行完毕之后要回调doLast的这部分闭包的代码实现。

熟悉Ant的读者，会觉得任务(Task)和Ant里的Target(目标)非常相似，其实没错，现在可以认为他们基本上相同。

再看gradle -q hello这段运行命令，意思是要执行build.gradle脚本中定义的名为hello的Task，-q参数用于控制gradle输出的日志级别，哪些日志可以输出被看到。

看到println 'Hello World!'了吗，他会输出Hello World!，通过名字相信大家已经猜出来了，它其实就是System.out.println("Hello World!")的简写方式。Gradle可以识别它，是因为Groovy已经把println()这个方法添加到iava.lang.Object，而在Groovy中，方法的调用可以省略签名中的括号，以一个空格分开即可，所以就有了上面的写法。还有一点要说明的就是在Groovy中，单引号和双引号所包含的内容都是字符串，不像Java中，单引号是字符，双引号才是字符串。

### Gradle Wrapper

Wrapper，顾名思义，其实就是对Gradle的一层包装，便于在团队开发过程中统一Gradle构建的版本，这样大家都可以使用统一的Gradle版本进行构建，避免因为Gradle版本不统一带来的不必要的问题。

在这里特别介绍的目的是因为我们在项目开发过程中，用的都是wrapper这种方式，而不是我们在1.1里介绍的自己下载ZIP压缩包，配置Gradle的环境的方式。Wrapper在Window下是一个批处理脚本，在Linux下是一个shell脚本，当你使用wrapper启动Gradle的时候，wrapper会检查gradle有没有被下载关联，如果没有将会从配置的地址（一般是Gradle官方库）进行下载并运行构建。这对我们每个开发人员是非常方便的，因为你不用去专门的配置环境了，只要执行wrapper命令，它会帮你搞定一切，这样方式也方便我们在服务器上做持续集成(CI)，因为我们不用在服务器上配置Gradle环境。

#### 生成wrapper

Gradle提供了内置的wrapper task帮助我们自动生成wrapper所需的目录文件，在一个项目的根目录，输入gradle wrapper即可生成。
```shell
$ gradle wrapper
:wrapper
        
BUILD SUCCESSFUL
        
Total time: 2.804 secs
        
This build could be faster, please consider using the Gradle Daemon: http://gradle.org/docs/2.5/userguide/gradle_daemon.html
```
生成的文件如下：
```shell
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
└── gradlew.bat
```
gradlew和gradlew.bat分别是Linux和Window下的可执行脚本，他们的用法和gradle原生命令是一样的，gradle怎么用，他们也就可以怎么用。gradle-wrapper.jar是具体业务逻辑实现的jar包，gradlew最终还是使用java执行的这个jar包来执行相关gradle操作。gradle-wrapper.properties是配置文件，用于配置使用哪个版本的gradle等，稍后会详细讲解。

这些生成的wrapper文件可以作为你项目工程的一部分提交到代码版本控制系统里(git)，这样其他开发人员就会使用这里配置好的统一的gradle进行构建开发。

#### wrapper配置

当我们在终端执行gradle wrapper生成相关文件的时候，可以为其指定一些参数，来控制wrapper的生成，比如依赖的版本等。

参数名|说明
-----|------
--gradle-version|用于指定使用的gradle版本
--gradle-distribution-url|用于指定下载gradle发行版的url地址


使用方法如gradle wrapper --gradle-version 2.4这样，这样就意味着我们配置wrapper使用2.4版本的gradle，它会影响gradle-wrapper.properties中的distributionUrl的值，该值的规则是http\://services.gradle.org/distributions/gradle-${gradleVersion}-bin.zip

如果我们在调用gradle wrapper的时候不添加任何参数呢，那么就会使用你当前Gradle的版本作为生成的wrapper的gradle version。例如你当前安装的gradle是2.5版本的，那么生成的wrapper也是2.5版本的。

#### gradle-wrapper.properties

该配置文件是gradle wrapper的相关配置文件，我们上面执行该任务的任何配置都会被写进该文件中，现在我们来看看该文件的配置字段：

字段名|说明
---------|------
distributionBase|下载的gradle压缩包解压后存储的主目录
distributionPath|相对于distributionBase的解压后的gradle压缩包的路径
zipStoreBase|同distributionBase，只不过是存放zip压缩包的
zipStorePath|同distributionPath，只不过是存放zip压缩包的
distributionUrl	|gradle发行版压缩包的下载地址

我们比较关注的就是distributionUrl这个字段了，这个决定你的gradle wrapper依赖哪个gradle版本，一般生成的都是这样的https\://services.gradle.org/distributions/gradle-2.5-bin.zip，我通常都会把bin改为all，这样在开发过程中，就可以看到gradle的源代码了。

基于Gradle 2.5默认生成的gradle-wrapper.properties如下：
```shell
#Wed Sep 16 23:14:52 CST 2015
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.5-bin.zip
```
distributionUrl是下载gradle的路径，如果运行./gradlew的时候一直卡着不动，可能是因为官方的gradle地址被墙了，建议使用翻墙工具试试，或者把该地址换成别的镜像地址。

#### 自定义Wrapper Task

前面我们讲了，gradle-wrapper.properties是由wrapper task生成的，那么我们是否可以自定义配置该Wrapper task来达到我们配置gradle-wrapper.properties的目的呢，答案是肯定的。我们在我们的build.gradle构建文件中录入如下脚本：
```groovy
task wrapper(type: Wrapper) {
    gradleVersion = '2.4'
}
```
这样我们再执行gradle wrapper的时候，就会默认生成2.4版本的wrapper了，而不用使用--gradle-version 2.4进行指定了。同样你也可以配置其他参数：
```groovy
task wrapper(type: Wrapper) {
    gradleVersion = '2.4'
    archiveBase = 'GRADLE_USER_HOME'
    archivePath = 'wrapper/dists'
    distributionBase = 'GRADLE_USER_HOME'
    distributionPath = 'wrapper/dists'
    distributionUrl = 'http\://services.gradle.org/distributions/gradle-2.4-all.zip'
}
```
以上是我自己配置的一些值，也可以修改生你自己的。

### Gradle日志

在这里介绍单独介绍Gradle日志的目的是为了便于我们在遇到问题的时候，能够根据日志信息分析和解决问题。Gradle的日志和我们自己Java、Android差不多，也分一些级别，用于分类显示日志信息，这样我们只需根据不同的情况显示不同类别的信息，不至于被大量的日志搞的晕头转向。

#### 日志级别

上面提了Gradle的日志级别和大部分我们使用的语言的差不多，除了这些通用的之外，Gradle又增加了QUIET和LIFECYCLE两个级别，用于标记重要以及进度级别的日志信息。

级别|用于
-----|----
ERROR|错误消息
QUIET|重要消息
WARNING|警告消息
LIFECYCLE|进度消息
INFO|信息消息
DEBUG|调试信息

上面的表列明确的列出了6种日志级别以及他们的作用，那么现在我们就看下怎么样使用他们。要使用他们，显示我们想要显示级别的日志，就要通过命令行选项中的日志开关来控制。
```shell
#输出QUIET级别及其之上的日志信息
$ gradle -q tasks
#输出INFO级别及其之上的日志信息
$ gradle -i tasks
```
以下列出所有通过命令行开关选项可以控制的级别级别，在命令行里只需加上这些选项即可控制使用。

开关选项|输出的日志级别
-------|--------------
无选项|LIFECYCLE及其更高级别
-q或者 --quiet|QUIET及其更高级别
-i 或者 --info|INFO及其更高级别
-d or --debug|DEBUG及其更高级别，这一般会输出所有日志

#### 输出错误堆栈信息

在使用Gradle构建的时候，难免会有这样或者那样的问题导致你的构建失败，这时就需要你根据日志分析解决问题。除了以上的日志信息之外，Gradle还提供了堆栈信息的打印，用过Java语言的相信大家都会很熟悉错误堆栈信息，他能帮助我们很好的定位和分析问题。

默认情况下，堆栈信息的输出是关闭的，需要我们通过命令行的堆栈信息开关打开它，这样在我们构建失败的时候，Gradle才会输出错误堆栈信息，便于我们定位分析和解决问题。

命令行选项|用于
----|-----
无选项|没有堆栈信息输出
-s 或者 --stacktrace|输出关键性的堆栈信息
-S 或者--full-stacktrace|输出全部堆栈信息

一般推荐使用-s而不是-S，因为-S输出的堆栈太多太长，非常不好看，而-s反而比较精简，可以定位解决我们大部分的问题。

#### 自己使用日志信息调试

在编写Gradle脚本的过程中，我们有时候需要输出一些日志，来验证我们的逻辑或者一些变量的值是否正确，这时候我们就可以使用Gradle提供的日志功能。

通常情况下我们一般都是使用print系列方法，把日志信息输出到标准的控制台输出流（它被Gradle定向为QUIET级别日志）。
```groovy
println'输出一段日志信息'
```
除了print系列方法之外，你也可以使用内置的logger更灵活的控制输出不同级别的日志信息。
```groovy
logger.quiet('quiet日志信息.')
logger.error('error日志信息.')
logger.warn('warn日志信息.')
logger.lifecycle('lifecycle日志信息.')
logger.info('info日志信息.')
logger.debug('debug日志信息.')
```
logger说是内置，这里其实是调用的Project的getLogger()方法获取的Logger对象的实例。

### Gradle命令行

Gradle命令行单独抽出来一小结的目的是想提倡大家尽可能的使用命令行，而不要太依赖于各种IDE，虽然IDE很方便，但是如果你换了一家公司，不适用这个IDE呢？如果让你做自动构建呢？可是没有IDE给你用的，全部都是基于命令行的。这个就像我们第一次学习变成语言时老师没说让你用IDE，而是直接用记事本或者其他文本工具写，目的就是让我们不要太依赖第三方工具，这种才能不变应万变。那么IDE该不该用，有没有必要，这个是肯定的，一定要用，因为他能提高你工作效率，但是用之前你要知道不借助IDE如果做一件事件，比如执行Gradle一个Task，在Android Studio下很简单，双击那个Task就可以执行了，但是如果没有Android Studio，你也要知道如何在命令行下如何运行它。我们要知其所以然，不然你再做几年也是这样啦。

#### 记得使用帮助

命令行下的工具都有命令，刚开始我们不会用或者不知道有什么命令或者参数，这没事，但是我们可以通过帮助来了解，基本上所有的命令行工具都有帮助，查看帮助的方式也很简单，基本上都是在命令后跟-h或者--help，有的时候会有-?,以Gradle Wrapper为例：
```shell
./gradlew -?
./gradlew -h
./gradlew --help
```
#### 查看所有可执行的Tasks

有时候我们不知道如何构建一个功能，不知道执行哪个Task，这时候就需要查看哪些Task可执行，都具备什么功能，通过运行./gradlew tasks命令，输出如下：
```shell
:tasks
     
------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------
     
Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]
     
Help tasks
----------
components - Displays the components produced by root project 'flysnow'. [incubating]
dependencies - Displays all dependencies declared in root project 'flysnow'.
dependencyInsight - Displays the insight into a specific dependency in root project 'flysnow'.
help - Displays a help message.
model - Displays the configuration model of root project 'flysnow'. [incubating]
projects - Displays the sub-projects of root project 'flysnow'.
properties - Displays the properties of root project 'flysnow'.
tasks - Displays the tasks runnable from root project 'flysnow'.
     
To see all tasks and more detail, run gradle tasks --all
     
To see more detail about a task, run gradle help --task <task>
     
BUILD SUCCESSFUL
     
Total time: 2.321 secs
     
This build could be faster, please consider using the Gradle Daemon: http://gradle.org/docs/2.5/userguide/gradle_daemon.html
```
从输出我们可以看到，Gradle会以分组的方式列出Task列表，比如构建类的有init，wrapper；帮助类的有help、tasks等。

#### gradle help 任务

除了上面我们说的每个命令行都有帮助外，Gradle还内置了一个help task，这个help可以让我们了解每一个task的使用帮助，用法是 ./gradlew help --task <task name>，比如 ./gradlew help --task tasks，就可以显示tasks任务的帮助信息：
```shell
:help
Detailed task information for tasks
    
Path
     :tasks
    
Type
     TaskReportTask (org.gradle.api.tasks.diagnostics.TaskReportTask)
    
Options
     --all     Show additional tasks and detail.
    
Description
     Displays the tasks runnable from root project 'android-gradle-book-code' (some of the displayed tasks may belong to subprojects).
    
Group
     help
    
BUILD SUCCESSFUL
```
从帮助信息我们可以看到这个task有什么用，他是什么类型，属于哪个分组，有哪些可以使用的参数，比如这里就有--all参数，可以查看很多额外的详细信息。

#### 强制刷新依赖

我们一个功能不可避免的会依赖很多第三方库，像Maven这类工具都是有缓存的，因为不可能每次编译的时候都要重新下载第三方库，缓存就是这个目的，先使用缓存，没有再下载。默认情况下Maven这类工具会控制缓存的更新，但是也有例外，比如Version一样，但是里面的代码变了；还有就是联调测试时使用的snapshot版本。上两种情况我们在实际项目中都遇到过，最后就是通过强制刷新解决的。强制刷新很简单，只要在命令行运行的时候加上--refresh-dependencies参数就可以，这是IDE很难做到的（需要你了解配置），所以命令行的优势就体现出来了，非常简单。
```shell
./gradlew --refresh-dependencies assemble
```
其他还有很多有用的命令、参数以及Tasks，就不一一介绍了，大家可以通过上面讲的两种帮助方法来了解熟悉。

#### 多任务调用

有时候我们需要同事运行多个任务，比如在执行Jar之前先进行clean，那么我们就需要先执行clean对class文件清理，然后再执行jar生成一个jar包。通过命令行执行多个任务非常简单，只需要按顺序以空格分开即可，比如./gradlew clean jar，这样就可以了，有更多的任务时，可以继续添加。

#### 通过任务名字缩写执行执行

有的时候我们的任务名字很长，如果在执行的时候全部写一遍也挺费时间，为此Gradle提供了基于驼峰命名法的缩写调用，比如connectCheck，我们执行的时候可以使用./gradlew connectCheck,也可以使用./gradlew cc这样的方式来执行。

以上内容节选自《Android Gradle权威指南》，更多关于《Android Gradle权威指南》的内容请参见[http://yuedu.baidu.com/ebook/14a722970740be1e640e9a3e](http://yuedu.baidu.com/ebook/14a722970740be1e640e9a3e)

