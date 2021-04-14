# 01_maven企业级实战

### 01_部分学员的maven基础问题以及我们的学习目标

1、市面上现有的maven视频

（1）培训机构就业班里的maven视频

（2）没讲清楚：架构、原理

（3）入门上手：简单能用起来，跟真正的企业级实战程度差了不知多远 

2、市面上现有的maven书籍 

（1）《Maven实战》：经典书籍，唯一一本

（2）讲解全面

（3）300多页，不见得有几个人已经全部看过两遍，而且100%掌握

（4）站在技术讲解的角度来写，出于技术完整性来讲解，不是按照从入门到企业实际应用场景如何使用各种功能的角度来讲解，很多人一遍看不明白，得看两三遍 

3、架构班内同学的maven基础 

（1）不排除少数工作多年的学员，在有一定规范的公司里工作过，对maven企业应用实战的能力掌握的非常好了，甚至都不需要听这门课程

（2）但是还有相当一部分的学员，刚工作1~3年，或者是大数据方向，或者是所在公司一直没有较为规范的对maven进行企业级应用

（3）对maven一知半解，简单能参照公司里其他人的样子，配置一下，依葫芦画瓢，项目跑起来就好

（4）完全没有掌握对maven进行企业级应用，真正管理好你的工程项目 

4、如果不讲解这个课程的后果 

（1）maven是国内大多数公司的主流构建管理工具

（2）进公司第一件事情：jdk，eclipse，maven，git/svn

（3）工程师就罢了：参照别人，依葫芦画瓢，糊弄过去，能跑就行

（4）架构师？？？oh my god，你可是要带团队，从零开始搭建项目的，如果不精通maven，那你就把工程做的一团糟，很low，瞎搞，没有任何规范和企业级应用的既视感，甚至也许会弄出一些低级笑话

（5）更有甚者，你别的技术掌握很好，但是进公司这个最基础的技能一知半解，别人会对你很质疑，我都不愿意承认你是我的学生

（6）后面我们是完全按照公司标准来做项目，maven各种功能大量应用，如果不深入掌握这个课程，你到了后面也会跟着我的视频依葫芦画瓢，也许出了问题自己都没法解决 

5、学习的目标 

（1）从此以后进公司第一件事情，看公司项目里的各种maven配置，并且配置自己的maven环境，不再虚！

（2）从此以后使用各种依赖或者插件，进行配置的时候，不再照着网上抄，依葫芦画瓢，弄的迷迷糊糊的，不再虚！

（3）从此以后看各种开源项目的pom.xml，不再觉得无比复杂，轻松看明白，不再虚！

（4）从此以后作为一个架构师，进公司第一事情，搭建maven私服，用maven进行企业级的规范工程管理，不再虚！

### 02_在没有maven的“原始社会”有多痛苦

没有maven的原始社会中，我们程序员的日常工作流程如下。。。。。。

1、每天都要编写自己负责的功能模块的代码，然后编写对应的单元测试，接着运行单元测试，生成单元测试覆盖率的报告，发送给自己的leader 

2、注意，如果你的项目遇到了需要使用的第三方jar包，你需要自己去寻找对应版本的jar包，然后添加到你的工程里面去 

我们的每一个工程，都是要依赖大量的其他第三方的项目的，比如最经典的，ssh框架的整合，spring mvc spring mybatis。这个时候就要把对应框架的jar包给从网上下载下来，然后copy到我们自己的工程的lib目录下面去。 

很麻烦，一个工程，依赖包太多了，可能多达数十个，搞一大堆 

然后就要开始尝试跑，依赖能不能放在一起整合在一起，跑通，这个时候还会出现各种缺少依赖jar包。你依赖了spring，spring可能又依赖了个什么什么包，此时直接运行还会报错，class not found exception，可能又要去下载更多的包放进来，直到不再报错。 

还有的时候，可能还会出现一些依赖冲突，比如你依赖了个什么mybatis，还依赖了spring，spring和mybatis都依赖了相同的一个项目，打个比方log4j，但是版本还不一样。这个时候你还得自己手动选择一个较高的版本，解决一些依赖的冲突问题。 

3、接着可能每隔几天都需要将自己开发好的代码进行编译、打包，然后在测试环境进行部署，等待对应的测试人员来测试 

每天都会写一些代码，还会写一些测试，但是每隔几天，可能一块功能开发好了，这个时候就要部署到集成测试环境里面，跟其他人开发的代码集成在一起，一起测试一下，能不能跑通。 

集成测试完了以后，先要编译代码，然后打包，手工部署 

如果测试过程中，出现了各种bug，再次修改代码，再次手工编译，手工打包，手工部署 

以此类推，直到集成测试通过 

接着呢，又是QA测试，测试工程师，来测试java工程师开发出来的系统。又是手工编译，手工打包，手工部署到QA测试环境。测试的过程中，有任何的bug，又是返工，重新修改代码，测试，然后手工编译，手工打包，手工部署到QA测试环境。 

部署也很坑，tomcat，每次部署，先停一下，然后弄个war包，放进去，把老的war包给干掉，新的war包放进去，tomcat启动 

每次要部署到某个环境的tomcat里面的时候，还得弄个终端软件，ssh连接到那个环境的服务器上面去，在那个服务器上的tomcat里面部署。 

4、再次注意，可能你一个大型的系统有很多个工程，每个工程的众多依赖包都需要你去手工管理，如果升级某个依赖，可能需要对几十个工程的jar包进行手工替换 

实际上来说，对于一些大型的erp系统，一些大型的oa系统，或者大型的银行系统，电信系统 

2010年以前，说实话，整个软件行业，技术还是比较low一点的 

2007，2006，2008 

一般都是一个单块应用，但是得拆分啊，可能是一个系统，拆分成了多个模块，每个模块是一个工程，最后会将多个工程的jar包放到一个统一的web工程里面去，让web工程去运行 

每个模块对应的工程，就是一个工程师，几个工程师在玩儿，在开发代码 

erp，电信系统，几十个工程，甚至上百个工程 

那个年代，不堪入目，很麻烦，纯手工，效率很低下，经常出问题 

5、同时注意，可能整个团队经常要对几十个工程进行繁琐的打包和部署 

6、日复一日，年复一年，项目做好了，接着又是将项目中所有工程，都进行代码编译，然后构建成一个一个的发布包，然后依次将几十个工程部署到生产环境 

程序员的痛苦在于。。。。。。 

1、你可能需要手工运行数十个，甚至数百个，上千个的单元测试，还要想办法弄出来一份完整的单元测试覆盖率的报告 

2、你可能需要手工管理多个工程中复杂的依赖jar包，包括后期的可能依赖冲突调解以及jar包版本升级 

3、你可能需要手工对多个工程进行繁琐的编译、构建发布包 

4、你可能需要频繁的手工部署发布包到各种环境下 

大量的手工，手工，手工，各种繁琐而且重复的操作，浪费了我们大量的时间，而且很容易出错 

这，就是没有maven这种工程管理工具的痛苦原始社会！

### 03_我们为什么需要maven：让依赖与构建完全自动化

如果我们有了maven之后，程序员的生活会是下面这样的。。。。。。

1、每天过来上班，先写代码，然后写单元测试，接着运行一个maven命令，只见哗啦，哗啦，哗啦，成百上千个单元测试就这么自动运行了，自动就出来一份完整的单元测试覆盖率的报告，以及单元测试运行错误的一份报告 

2、如果一个系统有多个工程，那就用maven来把多个工程集成在一起 

3、对于依赖，不用自己手工管理，简单用maven配置一下依赖，比如说你需要spring mvc，spring，mybatis几种依赖。然后用一个统一的父工程约束所有工程的依赖版本，所有的依赖下载、版本调解、版本升级等繁琐的事情全部由maven自动搞定 

比如说你的某一个依赖，spring还依赖了一个log4j，maven会自动给你再下载一个log4j，不用你自己去管，还缺失了哪些依赖 

不再需要自己各种手工上网下载jar包，放到lib目录下，解决各种jar缺失，jar包冲突 

4、编译+打包+发布？不用自己手工搞了，简单运行一下maven的命令，只见maven哗啦，哗啦，哗啦，就给你编译好代码，按照规定的格式打成发布包，然后直接连接到对应环境的web容器，给你自动发布了上去 

5、如果要对数十个工程统一进行编译、打包和整合，发布，一个命令，所有工程自动化给你搞，自动化编译、打包，所有的包给你整合在一起，最终的一个发布包就一键部署 

简单来说，用了maven之后，在依赖管理、构建管理、模块化拆分管理，全部自动化完成！ 

maven的前身 

1、make：最原始的构建工具，不能跨平台 

2、ant：曾经没有maven的时候，流行过一段时间，但是手工配置的语法繁琐，而且需要一次又一次的重复，另外依赖管理还要借助ivy来完成，工作量还是有点大。。。 

3、maven：自动化。。。，目前是最有影响力的工程管理工具 

4、gradle：google发布，不再基于xml来进行配置，而是基于DSL语言来进行构建管理，语法功能更加强大，android这块用的较多，同时国外一些开源项目，比如spring也开始用gradle来管理，国内也有少数公司在尝试使用 

5、说句实话：未来也许是maven和gradle并存的一个事情，但是近几年，主要还是maven 

用轻松的语言给大家描述了一下，实际上maven里面包含的东西是很多的，接下来，就用完全符合企业实战应用的场景模式，来给大家一一讲解maven的各种功能，每个功能都会带出来在公司里是在什么场景下使用的。 

提一点，很多视频课程，完全参照着书本方式去组织课程内容和讲解的 

语言，文字 

文字，不需要语言 

文字的意义在于说，可以用比较少的信息量，用比较精炼的方式，以一个结构化的思维把一个知识给你组织清楚，告诉你怎么回事 

写书，看书，看书，有局限性，没有办法说真正生动的，甚至是按照实战场景的方式给你去讲解是怎么回事 

导致很多人看书没有兴趣看下去，枯燥，看不太懂 

视频，语言，有情感，有情绪，我调动一下大家的积极性

然后有综合性的展现方式，可以打字，说话，画图，写代码，讲解，360度立体式的这么一个东西

### 04_在windows上完成maven的安装

1、确保安装了JDK

（1）点击Java 8的安装程序：jdk-8u151-windows-x64.exe，自己选择一个合适的目录一步一步安装即可，会安装jdk和jre两个东西

（2）在windows上配置JAVA_HOME环境变量，值应该是java的安装目录；配置Path环境变量，在里面加入%JAVA_HOME%/bin

（3）验证java是否安装成功：打开windows命令行，运行java -version

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0401.png)

2、在官网下载maven的包 

我们下载的是：apache-maven-3.5.2-bin.zip 

3、解压缩maven的包到某个目录中 

4、配置maven的环境变量 

配置M2_HOME环境变量为maven的安装目录

在PATH环境变量中，加入%M2_HOME%\bin 

5、执行mvn -v命令，检查安装是否完成 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0402.png) 

6、设置MAVEN_OPTS环境变量 

maven也是用java写出来的项目，也是要启动jvm来运行maven代码，进而执行各种操作的。因此maven自身的jvm内存大小也是要关注的，如果在构建特别大的项目时，可能会出现maven自身jvm内存不够，导致构建失败，比如OOM的异常。 

设置MAVEN_OPTS环境变量，就是设置maven的jvm参数，可以设置为-Xms128m -Xmx512m。 

7、设置maven的配置文件位置 

maven有一个重要的配置文件，就是settings.xml，这个文件默认是在%M2_HOME%/conf目录下面的，但是如果你升级maven的版本，那么可能导致新的安装包的settings文件又是一片空白。 

所以一般maven的配置文件都会放在当前用户的目录下的~/.m2/settings.xml中，这样就是对当前用户有效。 

（1）mvn help:system 

会自动下载一大堆东西，然后的话呢，就会在你当前windows登录用户的，c盘下面的用户文件夹，里面有一个当前登录账号的目录，里面的就是会自动创建一个.m2目录出来 

（2）然后将maven安装目录里的conf目录下的settings.xml配置文件拷贝到.m2目录里去，就ok了，作为以后maven全局唯一的配置文件

### 05_maven高效率的初体验：快速创建一个工程以及执行构建

1、使用maven快速创建一个工程 

为了加快速度，在settings.xml中加一段配置，用国内阿里云的镜像仓库去下载各种东西 

<mirror>

  <id>nexus-aliyun</id>

  <mirrorOf>*</mirrorOf>

  <name>Nexus aliyun</name>

  <url>http://maven.aliyun.com/nexus/content/groups/public</url>

</mirror> 

随便找一个地方创建一个目录（F:\development\workspace），然后在那个目录中执行下面的命令，基于maven创建一个工程：

mvn archetype:generate -DgroupId=com.zhss.maven -DartifactId=maven-first-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 

此时maven会在当前目录下，新建了一个目录，名称就是-DartifactId指定的名称。进入这个maven-first-demo目录，就可以看到maven自动给我们初始化好了一个工程对应的目录结构。 

2、maven的约定 

这就是基本的maven工程目录结构，其中src/main/java目录包含了这个项目的java源码，src/test/java目录包含了测试代码，pom.xml文件是maven的核心配置文件，是这个项目的Project Object Model。 

3、pom.xml初步介绍 

pom.xml文件是一个项目最核心的maven配置文件，包含了大量的信息，maven正是基于这里的配置信息来对工程进行构建等管理工作。一个最最基本的pom.xml文件如maven生成的pom.xml所示。 

<project>：pom.xml中的顶层元素 

<modelVersion>：POM本身的版本号，一般很少变化 

<groupId>：创建这个项目的公司或者组织，一般用公司网站后缀，比如com.company，或者cn.company，或者org.zhonghuashishan 

<artifactId>：这个项目的唯一标识，一般生成的jar包名称，会是<artifactId>-<version>.<extension>这个格式，比如说myapp-1.0.jar 

<packaging>：要用的打包类型，比如jar，war，等等。 

<version>：这个项目的版本号 

<name>：这个项目用于展示的名称，一般在生成文档的时候使用 

<url>：这是这个项目的文档能下载的站点url，一般用于生成文档 

<description>：用于项目的描述 

3、对项目进行打包 

可以认为源代码和测试代码都写好了，然后我们就要自动化运行测试用例+编译+打包，另外的话呢，这个项目有一个junit的依赖，我们期望的时maven可以自动给我们下载和管理这些依赖包 

使用mvn package命令，对一个工程进行构建，构建出来一个可以执行的java jar包。 

第一个发现，我们依赖了一个junit包，然后我们不需要自己手工去网上下载对应的jar包了，直接maven自动给我们下载了jar包，不再需要我们自己去管理依赖了，maven自动化管理依赖 

第二个发现，运行单元测试、编译、打包，自动化运行了单元测试的用例，自动化把你的java源代码编译成了.class文件，自动化把我们的代码打包成了一个jar包 

4、执行打好的jar包 

java -cp target/maven-first-app-1.0-SNAPSHOT.jar com.zhss.maven.App

### 06_一张图带你彻底看懂maven的体系结构

多说两句，上一讲，我们已经体验过了maven的高效率

这一讲，我们来说一下，这个maven命令一执行，各种下载依赖，自动化执行编译、打包（构建，build发布包） 

这一讲，你听明白了，后面的内容都是顺着这个往下讲的，而且你听后面的内容，都会觉得很简单 

但是这里的一个很大的问题在于，无论是培训机构的视频课程，还是说maven实战那本书籍，都漏了这一个部分 

项目中的pom.xml：依赖以及如何构建 

全局的maven配置，settings.xml 

maven的约定：各种约定目录，代码/资源/输出/测试 

mvn构建命令 

1、maven一定会去考虑settings.xml配置文件里的一些配置 

2、maven会去解析你的maven工程的pom.xml 

3、maven会去看你的pom.xml里面声明了哪些依赖 

4、maven会去本地的仓库里去找有没有那些依赖，比如有没有junit 

5、如果本地仓库没有junit，那么就会去远程仓库去找，下载junit。所谓的远程仓库里包含了几乎所有的依赖包，~/.m2/repository，本地仓库 

6、远程仓库下载到了junit以后，就会放到本地仓库，缓存起来，供你以后去使用，到%M2_HOME%/lib下面找，maven-model-builder-3.5.2.jar，https://repo.maven.apache.org/maven2/，maven的远程中央仓库 

下载依赖 -> 本地仓库 -> 远程仓库 

执行构建（清理、编译、打包）

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0601.png) 

### 07_基于m2eclipse插件完成eclipse与maven的整合

我们之前基于最原始的，纯手工的，命令行的方式来体验了一下maven是怎么用的，但是实际开发过程中，肯定不可能直接那样子搞的，有点不太现实

我们一般都是用ide去开发工程的，比如说eclipse，intellij idea，我们现在就要学习如何在eclipse上去安装一个eclipse的maven插件，就是m2eclipse，然后就可以在eclipse上面去开发和运行Maven工程 

1、安装eclipse 

（1）官网下载一个最新版本的eclipse就ok了，oxygen，最新的版本，我个人是建议大家，因为咱们这个架构班是一个持久战，可能大家要跟着我一起学习，一直搞到一年多的时间，这个过程中，我个人是建议大家，自己windows上的jdk，已有的maven，全部卸载掉，跟着我用我给的版本，一步一步全部重新装一遍，包括eclipse，保证所有东西版本一致，基本大家做实验就不会有什么太大的问题 

（2）eclipse-java-oxygen-1a-win32-x86_64.zip 

（3）找个目录，development\eclipse\java-oxygen，解压缩，就是一个eclipse目录，对里面的eclipse.exe创建一个快捷方式 

（4）双击eclipse就可以启动了 

（5）第一次启动要设置eclipse的workspace，选择F:\development\eclipse\eclipse-workspace 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0701.jpg) 

2、help -> install new software（可以不用了） 

3、add -> localtion -> http://m2eclipse.sonatype.org/sites/m2e -> 下一步下一步直接安装即可，默认会卸载掉一个eclipse自带的和maven整合的插件，然后我们最新下载的m2eclipse这个插件（可以不用了） 

4、直接用最新版的eclipse自带的和maven整合的插件即可，不然重新装一个有点画蛇添足的感觉，而且出现两个maven插件，有点奇怪 

5、不要使用内嵌的maven，preferences，配置maven的repository和settings地址 

一般来说，eclipse都会自带一个maven，而不会去使用我们在本地安装的那个maven，所以此时要配置eclipse使用我们安装的maven，而不是自己带的那个maven 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0702.jpg) 

如下图，installations里面，默认的就是一个eclipse内嵌的一个maven，这个不ok，如果你不做任何设置的话，会导致eclipse直接用自己的maven，虽然项目可以跑，但是。。。你对我们自己安装的那个maven做的任何设置都对eclipse无效。。。 

在add里面加一个自己本地安装的maven，然后确认一下user settings使用的配置文件是~/.m2/settings.xml，本地仓库目录要确认一下，用的是我们自己的那个~/.m2/repository

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0703.jpg) 

### 08_在eclipse上使用maven开发第一个hello world项目

1、在eclipse中创建一个maven项目 

new -> maven -> maven project -> maven archetype quickstart 

2、设置一下项目的坐标 

groupId：com.zhss.maven

artifactId：maven-helloworld

version：1.0-SNAPSHOT

package：com.zhss.maven 

在pom.xml中将junit的依赖从3.8.1版本修改成4.12版本 

在eclipse开发maven项目的话，每次你保存pom.xml，eclipse就会让maven自动去解析pom.xml，然后下载你最新声明的依赖，立即从远程仓库下载到本地仓库 

但是如果纯手工通过archetype去创建maven工程的话，是需要在mvn test package打包的时候，才会去检查pom.xml，才会去下载依赖 

在eclipse，window -> preferences -> 搜索font -> color and font -> basic -> text font，设置字体为courier new 

3、编写代码 

src/main/java 

package com.zhss.maven; 

public class HelloWorld { 

​     public String sayHello(String name) {

​         return "hello, " + name;

​     }     

​     public static void main(String[] args) {

​         System.out.println("hello world......"); 

​     }     

} 

4、编写测试代码 

package com.zhss.maven; 

import static org.junit.Assert.assertEquals; 

import org.junit.Test; 

public class HelloWorldTest { 

​     @Test

​     public void testSayHello() {

​         HelloWorld helloWorld = new HelloWorld();

​         String result = helloWorld.sayHello("leo");

​         assertEquals("hello, leo", result); 

​     }     

} 

5、用eclipse执行maven命令 

右击eclipse工程 -> run configurations -> maven build -> new -> 

maven-helloworld

选择工程目录中的maven项目

clean package 

run运行这个maven命令 

很多同学对我讲课过程中遇到的问题是怎么排查的很感兴趣 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\0801.jpg) 

windows -> preferences - java - installed jres -> 默认用的时jre目录 -> 修改为 jdk目录 

右击工程，build path -> configure build path -> 将用的jdk修改为刚才设置的那个jdk就可以了

### 09_案例背景引入：办公自动化OA系统 

我们接下来其实就是正式开始进入maven的各种功能的讲解了 

最最老土，最最老生常谈，培训机构最最喜欢用的OA 

为什么用OA，因为这个OA的概念相对来说比较好理解一些，不涉及什么业务领域的知识，银行系统，电信系统 

Offic Automatic，办公自动化，以前没有计算机的时候，办公都是各种纸张传递，在纸上写各种申请，文件，管理一些数据，比如公司里有哪些员工，有哪些部门，请个假，全部都是用excel去管理的。 

纸张办事效率低，人得面对面用纸去解决问题，一大堆纸，放在那儿，你要查找某份资料，很麻烦，可能要找半天，才能找到某个员工的资料 

计算机，在计算机上可以开发一些软件，就从国外开始出现了这个概念，OA，让公司内的各种常规的日常的办公，全部无纸化，自动化。员工数据的管理，不再是用excel去管理了，直接在一个java web系统里增删改查，很方便，查找某个员工的资料，不用去翻一大堆的纸，直接软件的搜索框里，输入姓名，直接几毫秒就给你查出来了。 

请个假，不用像以前那样，写个excel表格，打印出来，从一楼跑到五楼找领导，签个字，领导可能还不在，领带可能还出差了，人不再，没法给你签字。报销经费，签字。 

OA软件上，直接填写一个工单，请假工单，报销工单，直接系统就创建一份申请工单。领导也不用出现在你的面前签字，领导这个时候在日本出差，没问题，直接打开笔记本分的电脑，联网，查看你的申请工单，点击确认，就可以了。 

我们做这个案例怎么做，重点是做一个demo出来，里面涉及到一些spring mvc+spring+mybatis的框架整合，这些框架会有很多的依赖，方便我们去各种讲解。 

实际上里面的各种系统的业务模块的功能，全部简化，就是demo。就是很简单的功能，就是跟那些培训机构的demo简单一点。 

组织机构模块：部门和员工，增删改查，spring+mybatis整合起来，写出来service和dao层，里面可以对数据库里面的表进行增删改查 

权限管理模块：员工，角色，权限，任何一个系统，都有权限模块，就是去管理这个系统的各种用户能使用的权限。给一个service接口，他要依赖于组织机构模块，可以给一个员工授予某个角色。最终也可以将授权反应到数据库中去。可以授权，也可以取消授权。 

审批管理模块：员工，申请，审批。提供两个service接口，某个员工可以创建一个请假工单申请。然后领导可以对请假申请进行审批。也是可以反应到数据库中去。 

web工作台模块：web工程，将之前三个模块的功能全部包装成对外的http的接口，供前端来调用。 

工程化模块拆分 

每个模块拆分成一个工程，最终会用maven给整合起来 

实际上，你要是现在真的是在一个外包公司，做OA系统，给大型企业做OA系统，复杂，任何一个模块都很复杂。本来其实现在如果用maven的话，也都是每个模块拆分成一个工程，最终整合起来使用的。

### 10_maven依赖管理的基石性机制：坐标机制

从这一讲开始，正式开始讲解maven一个一个的知识点 

提一点点，大家也会看过一些其他的maven培训视频，我为什么还要自己重新讲解一遍呢？这个原因之前解释过了，这里我再提一点，就是哪怕是有些东西别的视频讲过了，但是我不太认可。 

做这个架构班的课程，市面上有的东西，我都会去看一下。他们讲解的细的程度，还有讲解的思路，我不认可。不够细，不够讲的透彻，不够讲的明白；还有讲解的思路，都是按照一种书本上的知识体系组织的结构去讲解的。我也不太认可。 

我个人后面架构班的思路，其实都是希望按照公司里，在做项目的过程中，如果需要学习一块技术，按照先入门，然后跟着项目开发进度走，一步一步顺着项目的进度，项目的需求，项目遇到的挑战，把这个技术一个一个的知识点给你讲解出来，结合项目来讲解，告诉你这个技术的这个知识点能够解决这个项目的什么问题。 

我个人觉得这样子讲解，效果是最好的，最理想的。 

而不是按照书本上的组织方式来讲解，比如maven生命周期，依赖管理，仓库，插件。书本是可以给你学习和参考，但是效果不是最理想的。 

大家有没有一个感触，就是自己掌握的最好的技术，和印象最深刻的，永远是那些自己公司里遇到过的项目中的挑战，促使你去学习和研究的东西，然后搞定了之后，解决了项目里的问题，这一块你会特别有底气，特别扎实，印象特别深刻。 

反之，如果你学习了很多的东西，各种看书，各种看视频，结果没有跟最实际的项目结合起来，导致你后面发现自己印象很淡薄，甚至都忘了。 

1、maven坐标的介绍 

每个maven项目都有一个坐标 

groupId + artifactId + version + packaging + classifier，五个维度的坐标，唯一定位一个依赖包 

任何一个项目，都是用这五个维度唯一定位一个发布包 

实际上后面两个维度较为少用，99%的场景下，唯一定位一个依赖的就是三个维度，groupId + artifactId + version 

几乎所有你需要使用的依赖包的各个版本，都在maven的中央仓库里 

简单来说，就是在pom.xml里配置你需要的依赖的坐标，然后maven自动从中央仓库下载后缓存到你本地仓库里 

打包的时候，可以通过插件直接将本地仓库中的依赖打入你的jar包中，形成一个完整可用的发布包 

而我们自己的maven项目，也可能要发布到maven仓库中，给别的项目使用，因此maven是强制性要求每个项目都要定义一个坐标的 

2、企业级的坐标设置 

groupId：不是你的公司或者组织，但是以你的公司或者组织的官网的域名倒序来开头，然后加上项目名称 

比如说，你现在在百度公司，www.baidu.com，公司里任何一个项目的开头，就可以用com.baidu来打头 

或者说，你在阿里巴巴，www.alibaba.com，公司里任何一个项目的开头，就是可以用com.alibaba来打头 

然后你可能会说，我的公司有点low，连域名都没有，网站都没有，比如你的公司叫中华石杉，没有网站，一般是这样，国内会用自己公司名字的拼音的缩写，或者是公司名称的英文的简写，加上com来组成groupId 

com.zhss 

咱们这个架构班里所有的课程，所有的工程，groupId都是com.zhss打头 

maven-helloworld是属于一个maven这个课程里的一个demo项目，com.zhss.maven 

接下来，我们不是要开始做一个oa案例，com.zhss.oa 

com.zhss + 项目名称，或者是系统名称 

com.zhss.hr

com.zhss.finance

com.zhss.crm 

比如我们这个课程，是中华石杉做的，一般会用中文的拼音简写，就是zhss，然后我们虽然没有自己的网站，但是一般可以用常规的com来打头，同时我们当前讲解的是这个课程中的maven部分，因此可以认为是一个maven单独的项目 

所以最终我们的groupId，就是com.zhss.maven 

artifactId：项目中的某个模块，或者某个服务 

一般在一个大项目最终会打成一个war包，可能里面有几十个工程组成，上百万行代码，发布到一个tomcat里面去。一个一个的模块，每个模块基本就是一个工程，最终各个工程会被打到一个war包里去而已。artifactId就是这个项目里的某个模块 

com.zhss.oa，oa-organ，organ是缩写，organization

com.zhss.oa，oa-auth，auth是authorization

com.zhss.oa，oa-flow，flow就是流程的意思 

咱们这个架构师课程，整个思路，就是完全真实还原一个完整的大型电商网站的开发过程，演进过程 

第一个阶段：单块应用

第二个阶段：微服务化架构

第三个阶段：高并发架构+高可用架构+安全性架构+稳定性架构+伸缩性架构+扩展性架构

第四个阶段：亿级流量超大型架构

第五个阶段：企业级框架开发

第六个阶段：企业级中间件开发

第七个阶段：大数据架构 

你去任何一个公司，都可以说你作为架构师，搞起来公司java这一块项目的任何一块的架构 

我们会发现，一个系统不是由多个模块组成了，不是最终打成一个war包发布。一个系统是由多个服务组成，artifactId就变成了服务的名称。 

com.zhss.oa，oa-organ服务

com.zhss.oa，oa-auth服务

com.zhss.oa，oa-flow服务 

比如我们刚才写的第一个maven工程，就是个hello world工程，所以artifactId就是hello-world 

version：这个工程的版本号，在最后几讲的时候，我们会讲解maven里的企业级的版本管理，是怎么玩儿的 

packaging：这个工程的发布包打包方式，一般常用的就jar和war两种，java -cp执行一个jar包，war可以放到一个tomcat容器里去跑的web工程

classifier：很少用，定义某个工程的附属项目，比如hello-world工程的，hello-world-source工程，就是源码，可能是类似于hello-world-1.0-SNAPSHOT-source.jar这样的东西。 

3、设置了坐标的作用是啥？ 

很简单，你每一次一个工程写好了，都有版本，groupId+artifactId+version就成了这一次这个工程目前这个状态的唯一的标识和定位。 

一个groupId+artifactId+version，就定位了这个项目某个时间点的一个特定版本的代码，也就是一个特定的版本的代码的jar包。 

然后呢在maven世界里，特别是开源的那些项目，junit，spring，mybatis，都是在每个版本的代码开发好之后，设置好对应版本的坐标，主要是version。然后将这个版本的代码的jar包，上传到maven的中央仓库。供其他任何人来使用这个版本的代码的jar包。 

然后，我们在自己的maven项目里，就可以通过<dependency>加上那个依赖的坐标，groupId+artifactId+version，去声明我要用那个依赖的哪个版本的代码的jar包。maven通过这个坐标唯一定位到那个项目的那个版本的代码的jar包。从中央仓库下载下来，给你放到本地仓库里去。 

然后，你自己的maven项目为什么也要一个坐标，因为可能你写好了一个模块之后，你就会需要将这个版本的代码打成一个jar包，放到仓库里去，给公司里其他人去依赖和使用。只有你也有一个坐标，才能让别人唯一定位你的项目某个版本的代码的jar包，让maven下载了之后给别人用。 

坐标的思路，三步走 

（1）坐标有哪些元素

（2）坐标的每个元素都是什么意思，怎么设置

（3）设置了坐标之后的意义在哪儿1627098499  

### 11_企业实战场景1：开发组织机构模块&设置规范的企业级坐标

1、创建一个组织机构模块的工程 

进入咱们的第一个企业实战场景 

这个什么意思，比如我们在公司里面，开始接到了一个任务，比如你是个RD（研发工程师），接到了leader给的任务，就是开发一个组织机构模块 

你要做的第一件事情，就是自己去构建一个maven工程，然后参照我们上一讲说的那个意思，设置标准的，规范化的坐标 

在pom.xml里面声明各种你需要的依赖包，spring+mybatis整合，开发出对应的功能来，部门和员工这两个实体进行增删改查，做出来。写一下对应的单元测试。 

2、设置规范的坐标 

（1）在eclipse里面创建一个maven项目，坐标设置为如下所示： 

groupId: com.zhss.oa

artifactId: oa-organ

version: 1.0.0-SNAPSHOT 

groupId：com.zhss这个组织的oa项目 

artifactId：oa项目中的organization模块 

version：1.0.0-SNAPSHOT版本 

第三位是最小版本，一般如果是修复一些bug，或者作出轻微的改动，会累加第三位小版本； 

第二位是小版本，一般如果加入一些新的功能或者模块，或者做了一些重构，会累加第二位小版本； 

第一位是大版本，一般就是如果整体架构有特别的升级或者变化，才会累加第一位大版本。 

SNAPSHOT，就是当前这个版本下的快照版本，代表代码正在开发或者测试中，可以试用，但是没有经过完善测试，不会承诺非常稳定 

如果没有SNAPSHOT，那么就是说已经经过完善测试，是可以发布的稳定版本 

（2）再设置一下build path里面，将用的jdk设置一下 

（3）调整一下pom.xml里面的缩进格式，将junit版本修改为4.12，删除原先自带的App.java类和AppTest.java类 

3、整合需要的框架spring + mybatis 

（1）在pom.xml里放入对应的依赖 

4、配置文件 

这里说明一个知识点，之前说过，maven项目的约定，src/main/java是放源代码，src/test/java是放咱们的测试代码 

除了代码之外，通常来说，还有这个配置文件，一般maven约定是放在src/main/resources下面的。如果是测试代码要用的资源配置文件，是放在src/test/reousrces下面的。 

（1）新建一个src/main/resources目录 

（2）mybatis的配置文件，mybatis-config.xml，放在src/main/resources 

（3）安装MySQL数据库 

第一步，运行mysql安装文件，进行安装

第二步，设置中文字符集编码，最新版的mysql装完后都不用自己去设置了

第三步，确认一下mysql服务已经安装了

第四步，确认一下mysql的workbench在哪儿，创建一个快捷方式

第五步，创建一个oa数据库，create database oa 

（4）jdbc的配置文件，jdbc.properties，放在src/main/resources 

（5）spring配置文件，application.xml，放在src/main/resources下 

（6）log4j配置文件，log4j.properties，放在src/main/resources下   

（7）创建员工表和员工实体类 

CREATE TABLE `employee` (

 `id` int(11) NOT NULL,

 `name` varchar(45) DEFAULT NULL,

 `age` int(11) DEFAULT NULL,

 PRIMARY KEY (`id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='员工表' 

（8）编写mapper接口 

（9）编写mapper文件  

（10）编写service类 

（11）编写单元测试类 

（12）几个问题 

第一个问题，表要修改一下，给id勾上AI，让它自增长

第二个问题，就是classpath路径有问题

### 12_深入理解maven的各种复杂的依赖管理机制 

我们上一讲搭建了一个工程，而且重点是什么做了两件事情 

第一件事情：交给了大家如何在一个项目中去设置企业级的坐标 

第二件事情：在pom.xml里面加入了各种各样的依赖，搭建了一个spring+mybatis框架整合的这么一个模块，在模块里面也开发了一些功能，测试没有问题，打包没有问题 

我们来重点讲解一下依赖这个事情，maven到底是如何去管理依赖的，依赖管理的机制是较为复杂的，这里面都有哪些学问，声明了那些<dependency>元素之后，maven干了哪些事情 

1、用<dependency>可以引用任何你需要的依赖 

<dependency>

​     <groupId></groupId>

​     <artifactId></artifactId>

​     <version></version>

​     <type></type>

​     <scope></scope>

​     <optional></optional>

</dependency> 

在哪里找到你需要的依赖？ 

比如你感觉自己可能需要一个什么东西，你怎么知道要引入哪些依赖呢？

分两块来说，第一块，如果你是要用某个比较重要的技术，比如elasticsearch，spring，mybatis，等等，开源项目，直接到官网里面去，去找里面会告诉你如何在java里面引入maven依赖 

另外一块，是这样的，可能在你开发的过程中，也许你还需要一些小公司，比如log4j，slf4j，jackson等等这种东西，连官网都用去了，mysql connector驱动。直接上百度 

百度：maven + 你需要的依赖名称，比如log4j 

下面两个方式，走国外的网站去搜索，一般反正我比较少用，速度较慢 

sonatype nexus：http://repository.sonatype.org

mvnbrowser：http://www.mvnbrowser.com 

2、依赖声明的三要素，坐标 

groupId、artifactId、version，就ok了，type很少用 

声明了三要素，一个坐标唯一定位了一个依赖的某个版本的jar包，maven会自动到远程的中央仓库里面去，给我们去找，下载到本地来，在打包的时候，就会自动使用，编译的时候 

3、依赖范围 

<scope></scope> 

maven有三套classpath，classpath，就是项目中用到的各种依赖的类，jvm在运行的时候需要去classpath下面加载对应的类 

maven在编译源代码的时候有一套classpath；在编译测试代码以及执行测试代码的时候，有一套classpath；运行项目的时候，有一套classpath；依赖范围就是用来控制依赖包与这三种classpath的关系的。 

简单来说，不同的依赖范围，会导致那个依赖包可能在编译、测试或者打包运行的时候，有时候可以使用，有时候不能够使用 

compile：默认，对编译、测试和运行的classpath都有效。一般都是用这种scope 

test：仅仅对于运行测试代码的classpath有效，编译或者运行主代码的时候无效，仅仅测试代码需要用的依赖一般都会设置为这个范围，比如junit。一些测试框架，或者只有在测试代码中才会使用的一些依赖，会设置为test，这个的好处在于说，打包的时候这种test scope的依是不会放到最终的发布包里去的。减少发布包的体积。 

provided：编译和测试的时候有效，但是在运行的时候无效，因为可能环境已经提供了，比如servlet-api，一般就是这个范围，在运行的时候，servlet容器会提供依赖。servlet-api是用来开发java web项目的，可能你在开发代码和执行单元测试的时候，需要在pom.xml里面声明这个servlet-api的依赖，因为要写代码和测试代码。但是最终打完包之后，放到tomcat容器里面去跑的时候，是不需要将这个servlet-api的依赖包打入发布包中的，因为tomcat容器本身就会给你提供servlet-api的包。 

runtime：测试和运行classpath有效，但是编译代码时无效，比如jdbc的驱动实现类，比如mysql驱动。因为写代码的时候是基于javax.sql包下的标准接口去写代码的。然后在测试的时候需要用这个包，在实际运行的时候才需要用这个包的，但是编译的时候只要javax.sql接口就可以了，不需要mysql驱动类。一般我们声明mysql驱动的时候，不会设置为runtime，因为也许你开发代码的时候会用到mysql驱动特定的api接口，不仅仅只是用javax.sql。 

3、传递性依赖 

每个依赖可能又有其他的依赖，其他依赖又有其他的依赖，循环往复 

纯手工时期，可能就是先加入第一层依赖，然后报错；接着加入第二层依赖，再报错，继续加入第三层依赖；循环往复，极其麻烦 

在最初期，10年以前，做java项目的时候，用类似maven的工具相对较少，一般很多国内的项目，都是手动直接放一大堆jar包。先找一堆spring的jar包，然后尝试运行，发现报错，说缺失某某类，再去找那个项目的jar包，加进来，又说缺失某某类，循环往复。 

maven的传递性依赖，就是说会自动递归解析所有的依赖，然后负责将依赖下载下来，接着所有层级的依赖，都会成为我们的项目的依赖，不需要我们手工干预。所有需要的依赖全部下载下来，不管有多少层级。这个就是maven的传递性依赖机制，自动给我递归依赖链条下载所有依赖的这么一个特性。 

好比说我们依赖了junit，junit依赖了A，A又依赖了B 

我们对junit的依赖范围是test，junit对A的依赖范围是compile，那么我们对A的依赖范围是什么呢？ -> test 

比如说，我们依赖于A，是compile；A依赖于B，是test；我们对B的依赖范围是空，就是我们不会去依赖B，因为你自己想想都知道，A只有在测试的时候才会使用B。我们依赖A是生产用的，我们去依赖B干嘛？B是给A测试的。 

传递性依赖机制对依赖范围也是有影响的，比如下面的表格，第一列是一级依赖，第一行是二级依赖，传递性依赖会导致多级依赖的依赖范围交叉在一起，会有影响： 

|          | compile  | test | provided | runtime  |
| -------- | -------- | ---- | -------- | -------- |
| compile  | compile  |      |          | runtime  |
| test     | test     |      |          | test     |
| provided | provided |      | provided | provided |
| runtime  | runtime  |      |          | runtime  |

有可能依赖是不会传递的，就是可能有些多层级的依赖，是不会成为我们项目的依赖的 

4、依赖调解 

传递性依赖深入去讲解的，既然说maven会自动解析所有层级的依赖，给我们自动下载所有的依赖，但是可能会出现依赖冲突的问题 

比如A->B->C->X(1.0)，A->D->X(2.0)，A有两个传递性依赖X，不同的版本 

就产生了依赖冲突的问题，maven如何解决呢？依赖调解的机制 

此时就会依赖调解，就近原则，离A最近的选用，就是X的2.0版本 

如果A->B->X(1.0)和A->D->X(2.0)，路径等长呢？那么会选择第一声明原则，哪个依赖在pom.xml里先声明，就用哪个 

5、可选依赖 

<optional>true</optional> 

此时依赖传递失效，不会向上传递 

如果A依赖于B，B依赖于C，B对C的依赖是optional，那么A就不会依赖于C。反之，如果没有optional，根据传递性依赖机制，A会依赖于C。 

比如项目B是一个通用的数据访问平台，可以接elasticsearch，可以接hbase，可以接redis，有几个依赖，但是这些依赖可能是不可以同时使用的，此时就可以声明为optional不要向上传递 

那么如果项目A依赖项目B，此时就需要根据自己的需求，引入elasticsearch/hbase/redis的依赖，让B可以跑起来。比如说，A就依赖于使用B来访问elasticsearch，那么A自己就引入elasticsearch的依赖即可。B对elasticsearch，hbase，redis的依赖是不会自动传递给A的，需要A自己去声明。 

这个很少很少用 

总结一下 

（1）声明依赖

（2）找依赖，两个方案去找到你需要的依赖的maven坐标

（3）找到依赖以后配置依赖，三要素，坐标

（4）依赖范围，根据对依赖的使用场景，比如仅仅在test环境下使用，或者在运行时由运行环境来提供，可以给依赖设置不同的依赖范围

（5）传递性依赖，自动解析所有层级的依赖

（6）传递性依赖的多层级的依赖范围，交叉影响的结果

（7）依赖调解，碰到一个项目多个版本，启动根据两大就近原则来选择一个最合适的版本来使用

（8）可选依赖，不向上传递

1627098499 

### 13_企业实战场景2：一招教你搞定工作中最令人头疼的依赖冲突问题

1、什么叫做maven的依赖冲突问题？ 

大家先自己思考一下自己有没有遇到过，上一讲，我们不是说maven会自动依赖调解，说对已给项目不同的版本选择一个版本来使用 

但是如果maven选择了错误的版本呢。。。。。。 

某某某class not found

某某某方法() not found 

MyClass.doMethod() not found。。。。 

oh my god，这个依赖，我有声明啊，可能是我的某个一级依赖引入的二级依赖，或者三级依赖，但是为啥这个坑爹的二级或者三级依赖找不到呢。。。 

这个就是典型的工作场景里面的依赖冲突问题 

传递性依赖 + 依赖调解，导致了这个问题 

2、依赖冲突是如何产生的？ 

比如你依赖了A和B，此时A依赖了C-1.0，B依赖了D，D依赖了C-2.0 

X

 -> A

-> C-1.0

 -> B

-> D

 -> C-2.0 

X -> A -> C-1.0

X -> B -> D -> C-2.0 

此时就会导致的事情是，由于A -> C1.0是最短路径，所以会用C1.0 

但是坑爹的事情发生了，B依赖的是D，D依赖的C结果用了C-1.0版本 

D本来用的是C-2.0的一个方法，但是现在给D的时C-1.0的一个类 

比如C-1.0的类CClass.sayHello() 

C-2.0给类CClass加了一些方法，比如CClass.printHello()，同时也有CClass.sayHello()方法 

D调用了C-2.0里面的printHello()这个方法 

如果用的时C-1.0，把C-1.0的CClass提供给D去用，D去调用printHello()方法的时候，就会报错。。。 

C这个项目的CClass这个类的printHello()这个方法没有找到，not found的异常 

3、如何解决这样的依赖冲突？ 

很简单，一般来说，用最新的版本，因为新版本一般可以兼容旧版本，但是旧版本一定不会提供新版本的功能。一个开源项目是一定要提供向前兼容性的，否则就是太垃圾了，那你可以考虑换一个开源项目了。 

你不要让maven自动去选择C-1.0来使用，而是手动强制使用C-2.0，不就可以了么。。。。 

4、mvn depedency:tree这个命令 

mvn dependency:tree，用dependency插件的tree goal，执行，进行依赖链条分析 

我这边告诉大家，比如你发现C这个项目下报出了类似的问题，然后就用mvn dependency:tree这个命令看一下整个项目的依赖路径树，看看所有的依赖路径里，有哪几个版本的C项目，看一下，自己看看要用哪个版本的C，然后将其他的C版本的依赖全部手动排除掉 

A

 --C-1.0

B

 --D

  --C-2.0 

定位原因，因为A-C1.0最短，所以用了C-1.0，也就是旧的版本 

而大多数项目都是最新版本兼容旧版本，也就是说，用C-2.0的话，A依赖的C-1.0的代码是可以运行的 

所以此时要做的事情就是。。。 

在A下面 

<dependency>

<groupId>A</groupId>

<artifactId>A</artifactId>

<version>1.0</version>

<exclusions>

​          <exclusion>

​              <groupId>C</groupId>

​              <artifactId>C</artifactId>

​          </exclusion>

</exclusions>

</dependency> 

通过这个以后，你如果再次运行mvn dependency:tre 

就会发现依赖树变成下面这样了： 

A

B

 --D

  --C-2.0 

这样的话，就只剩下了一个C-2.0，那肯定是用C-2.0楼，无论是A还是D，都是用C-2.0了 

（1）maven工作中最常见的依赖冲突问题的现象？

（2）产生的原因是什么？

（3）解决的思路

（4）具体用什么命令和配置去解决

### 14_一张图让你看懂maven的多层仓库架构

我们这个maven课，我个人觉得最好的讲解思路是从依赖这块入手 

很多培训视频，上来做完一个demo之后，就是去讲解生命周期，插件，然后才是依赖 

我觉得，我们用maven，看到的第一件事情就是依赖这块，怎么找到依赖，怎么声明依赖，依赖管理机制，依赖冲突（企业场景内容），先讲解了依赖这一块 

依赖比较基础的一些内容都讲解完了，下面就要来进入比较重要的仓库这块，maven的依赖是从哪儿下载下来的？？？？ 

这个实际上是在公司里是非常重要的一块内容，因为实际上你平时去公司里开发，必须了解公司的一些maven仓库的内容和配置，否则你怎么配置你自己在公司里的开发环境呢？？ 

1、仓库的基本介绍 

正常来说，maven会用仓库来存储所有的依赖，不会说是每个项目都放一份依赖这样子 

如果我们不使用maven，那么我们就会在各种工程里建一个lib目录，里面放入这个工程的所有依赖，在打包的时候通常会用eclipse的fatjar这个插件，将工程打成一个胖jar，里面包含了所有依赖。 

但是如果公司内有几十个，甚至上百个工程，那么每个工程都要维护一份依赖，第一非常浪费空间，第二非常麻烦，每次升级某个依赖，可能要将几十个工程里的lib包下的依赖，先删除旧版本，再放入新版本，很麻烦。 

而maven的仓库，就是用来统一存放各种依赖的地方。哪怕是有几十个工程，但是每个工程如果有相同的依赖，那么那个依赖在仓库里只会存在一次，不会放在各个工程自己的lib包下。消除了重复，如果要升级某个依赖，直接在各个工程的pom.xml里升级依赖的版本即可，大家都会自动引用最新版本的依赖了。在仓库里所有依赖就放一次，然后每个工程在pom.xml里面声明对依赖的引用即可，打包的时候，可以将所有依赖打入发布包即可。 

2、仓库的布局结构 

自己windows本地的仓库，是在~/.m2/repository目录，就是maven的本地仓库 

那么依赖在maven仓库里是以什么方式存放的呢？其实maven仓库的布局结构其实很简单，比如我们依赖了一个项目，它的坐标如下： 

groupId：log4j

artifactId：log4j

version：1.2.15 

那么这个依赖的jar包在maven仓库里的布局，就是存放在log4j/log4j/1.2.15/log4j-1.2.15.jar，log4j/log4j/1.2.15就是目录，log4j-1.2.15.jar就是依赖对应的jar包。 

就这么简单，所以大家随时可以在仓库里根据坐标找到某个依赖的jar包。 

所以你声明依赖之后，maven实际上就是根据依赖的坐标，先到本地仓库里面去根据坐标去一层一层的定位依赖所在的目录里面有没有这个jar包，如果没有的话，就是到远程仓库里面去根据同样的布局去找依赖的jar包。 

3、一张图带你看懂maven的多层仓库架构 

maven仓库的大类分成本地仓库和远程仓库两种，如果你声明了一个依赖，那么在构建打包的时候，先会去本地仓库里找，这个本地仓库的地址默认就在用户主目录下面的~/.m2/repository目录下面，里面各个依赖的存放路径就是跟上面说的那样；如果本地仓库找不到，那么就会去远程仓库找，默认是去maven自己的中央仓库里找，maven的中央仓库里几乎涵盖所有的依赖；然后会将中央仓库里的依赖下载下来放到本地仓库，缓存起来，供下次使用。 

中央仓库是属于一种远程仓库，但是不只是有中央仓库一种远程仓库而已 

远程仓库还有私服和其他远程仓库，私服指的是在公司内部的局域网内，架设一个服务器，上面放一个公司内私有的仓库，就是私服；其他远程仓库，就是其他公司开放出来的仓库，比如java.net maven仓库，jboss maven仓库，等等。 

（1）本地仓库 

本地仓库，windows默认路径是~\.m2\repository，linux默认路径是/home/root/.m2/repository 

可以修改本地仓库的路径，在~/.m2/settings.xml配置文件里，可以设置： 

<localRepository>某路径</localRepository> 

（2）远程仓库 

如果本地仓库没有某个依赖，那么maven就会从远程仓库去下载，默认就是从中央仓库去下载 

（3）中央仓库 

maven有一个自带的超级pom.xml文件，里面配置了一个默认的中央仓库： 

<repositories>

​     <repository>

​         <id>central</id>

​         <name>Maven Repository Switchboard</name>

​         <url>http://repo1.maven.org/maven2</url>

​         <layout>default</layout>

​         <snapshots>

​             <enabled>false</enabled>

​         </snapshots>

​     </repository>

</repositories> 

所以如果你不做任何配置，那么当你声明了某个依赖之后，就是如果本地仓库没有，就会去maven自带配置的这个中央仓库去拉取，就是http://repo1.maven.org/maven2这个地址。 

（4）私服 

一般正经公司都会自己假设一个maven私服，也就是在公司局域网内部，假设一个服务器，上面放一个maven私有仓库。 

因为很多公司的服务器是不允许访问外网的，只能访问局域网，为了更好的安全性起见。而且依赖下载的速度也很快。 

此时的话，核心的思路，就是在本地配置远程仓库为私服，如果本地仓库没有，先去私服找，如果私服没有，再去中央仓库找。在中央仓库找到后，先缓存在私服中，然后再缓存在本地仓库中。 

此外，假设自己的私服，另外一个意义，就在于说，自己公司内部的一些发布包，可以放到私服上，供公司内的项目组之间使用。 

（5）其他远程仓库 

有些依赖可能在中央仓库没有，或者中央仓库的速度太慢，此时可能会用其他的一些远程仓库，比如jboss的仓库。java.net，google，codehaus，jboss，还有一些其他公司自己搞的Maven仓库，有少数的依赖包可能在中央仓库里找不到，只在其他仓库里。 

那么私服除了连接中央仓库，还可以连接其他远程仓库。 

（6）镜像仓库 

比如说，像中央仓库在国外，很慢的，直接从中央仓库下载的话，是很慢的 

所以一般国内的一些大型的互联网公司，阿里云，会搞一个镜像仓库，完全跟中央仓库一模一样的，代理了中央仓库所有的请求 

你可以直接从阿里云镜像仓库去请求，如果有就直接返回了，国内网络的速度很快的，上百倍；阿里云如果自己没有，就会去从国外的中央仓库去下载 

你就知道了什么啊，pom.xml里面声明了各种依赖之后，是怎么去找的，仓库，仓库的布局，多层仓库（本地、私服、镜像、中央、其他）。 

下面我们就要用几讲的时间，把整个maven的仓库架构搭建起来，让依赖的下载走我们图里画的那一套复杂的仓库架构1627098499

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1401.png)

### 15_企业实战场景3：为你的公司安装一套nexus私服仓库

我们之前讲解过了整个maven这块的仓库的架构，本地仓库是本来就有的，默认就在~/.m2/repository下面，也可以在settings.xml里面去配置，搭建一个私服。

搭建私服的话，一般maven这块都是用nexus去搭建的

1、nexus的安装和启动

从http://nexus.sonatype.org/downloads/下载最新版本的nexus

将bundle包解压缩后放在某个目录下，包含两个子目录

nexus-3.6.0-02：这里包含nexus运行需要的文件
sonatype-work：包含nexus的配置文件、日志文件、仓库文件

启动nexus，windows上cmd，进入到nexus的bin目录下，里面有一个nexus.exe这个文件，执行命令nexus.exe/run启动nexus服务，这块要等个三五分钟，启动很慢，直到出现下图才启动成功 

打开浏览器访问http://localhost:8081/

可以修改nexus的一些配置，有两个配置文件，一个是在bin目录下面的nexus.vmoptions，另外一个是etc目录下面的nexus-default.properties，里面可以修改端口号（默认是8081）

nexus的默认管理员账号和密码是：admin、admin123

ctrl+c可以停止nexus

预告一下，实际上我们平时就是在windows上做开发和测试，我们会搭建虚拟机作为测试的环境，后面的课程，然后每个阶段结束之后

比如说，我们把一个阶段，基于spring boot去开发一个完整的大电商系统，做完了，整个会在阿里云租用一套完全真实的生产环境，那个时候呢，我们就会去讲解如何在真实的生产环境中去配置nexus私服，git私有代码仓库

### 16_一张图带你看懂如何玩儿转nexus私服中眼花缭乱的各种仓库

1、nexus中的仓库 

nexus安装好之后本身就内置了一些仓库

包括四种仓库类型 

hosted：宿主仓库，这个仓库，是用来让你把你公司内部的发布包部署到这个仓库里来，然后公司内的其他人就可以从这个宿主仓库里下载依赖去使用 

proxy：代理仓库，这个仓库不是用来给你公司内部的发布包部署的，是代理了公司外部的各种仓库的，比如说java.net，codehaus，jboss仓库，最最重要的，就是代理了公司外部的中央仓库，但是这里其实可以修改为nexus连接的应该是国内的阿里云镜像仓库，阿里云去连接中央仓库 

其他的一些常用的仓库，java.net，jboss，googlecode，codehaus，这种仓库，现在都很少用了 

group：仓库组，其实就是将，各种宿主仓库、代理仓库全部组成一个虚拟的仓库组，然后我们的项目只要配置依赖于一个仓库组，相当于就是可以自动连接仓库组对应的各种仓库 

仓库的格式是maven1或者maven2，仓库的布局，我们之前给大家讲解过，布局格式，这里现在统一都是maven2 

最后还有仓库的状态和路径 

maven-central：这是maven中央仓库的代理仓库 

maven-releases：该仓库是个宿主仓库，用于部署公司内部的release版本的发布包（类似于1.0.0,，release的意思就是你的工程已经经过了完善的测试，单元测试，集成测试，QA测试，上生产环境使用了）到这个仓库里面，供其他同事在生产环境依赖和使用 

maven-snapshots：该仓库是个宿主仓库，用于部署公司内部的snapshot版本的发布包到这个仓库里（如果你的某个工程还在开发过程中，测试还没结束，但是，此时公司里其他同事也在开发一些工程，需要依赖你的包进行开发和测试，联调，此时你的工程的版本就是类似1.0.0-SNAPSHOT这样的版本），供其他同事在开发和测试的时候使用 

3rd party：该仓库是个宿主仓库，主要用来部署没法从公共仓库获取的第三方依赖包，比如说，你的公司依赖于第三方支付厂商的一个依赖包，那个依赖包不是开源的，是商业的包。那么你是没有办法从maven中央仓库获取的。此时，我们可能会自己手动从支付厂商那里获取到一个jar包，下载之后上传到私服里来，就放这个仓库里，3rd-party仓库 

maven-public：仓库组，上面所有release仓库都在这个仓库组内 

3、一张图带你看懂nexus中的仓库是如何使用的 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1601.png)

### 17_企业实战场景4：基于nexus为你的公司搭建一套企业级的仓库架构

maven-public仓库组：已有 

maven-central代理仓库：从直接代理maven中央仓库，修改为代理阿里云仓库，http://maven.aliyun.com/nexus/content/groups/public 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1701.jpg)

maven-snapshots宿主仓库：已有 

maven-releases宿主仓库：已有 

3rd-party仓库：需要手动创建 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1702.jpg)

 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1703.jpg)

 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1704.jpg)

再次配置maven-public仓库组，将3rd-party仓库加入其中 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\1705.jpg)

###  18_企业实战场景5：结合镜像机制将公司项目配置为强制从nexus私服下载

我们现在已经有了私服了，下一步就是要将公司中的项目配置为强制从公司内的私服来下载，不允许走外网，这样可以统一收口管理。毕竟nexus私服本身也是代理了各种中央仓库，直接用nexus私服就可以了。 

通常会在settings.xml配置文件中，为当前机器统一配置使用的私服仓库地址，而且一般都是直接用私服中的仓库组，在settings.xml中用profiles即可。 

<profiles>

​      <profile>

​            <id>nexus</id>

​        <repositories>

​             <repository>

​                 <id>nexus</id>

​                 <name>Nexus </name>

​                   <url>http://localhost:8081/nexus/content/groups/public</url>

​                 <releases><enabled>true</enabled></releases>

​                 <snapshots><enabled>true</enabled></snapshots>

​             </repository>

​        </repositories>

​        <pluginRepositories>

​             <pluginRepository>

​                 <id>nexus</id>

​                 <name>Nexus Plugin Repository</name>

​                    <url>http://localhost:8081/nexus/content/groups/public</url>

​                 <releases><enabled>true</enabled></releases>

​                 <snapshots><enabled>true</enabled></snapshots>

​             </pluginRepository>

​        </pluginRepositories>

​      </profile>

</profiles> 

<activeProfiles>

​      <activeProfile>nexus</activeProfile>

</activeProfiles> 

激活对应的profile，在项目执行构建的时候，就会将profile中的仓库配置应用到每个项目中去 

除此之外，我们还需要用mirror镜像机制，来强制要求所有对远程仓库的请求，全部通过镜像走私服。所谓的镜像，就是如果某个仓库A代理了另外一个仓库B，里面的内容完全一样，那么仓库A就是仓库B的镜像。 

你可以认为是阿里云仓库就是中央仓库的一个镜像，nexus私服配置成所有远程仓库的镜像 

国内知名的就是阿里云的镜像，如果是在自己家里开发，自己玩儿，建议的就是在settings里配置个阿里云的镜像，所有的依赖直接从国内的阿里云镜像下载，速度很快，不要直接从国外的maven中央仓库下载。 

但是阿里云的镜像，一般也就自己开发一些demo项目，在家里用用了，跟企业级应用不搭边。 

配置需要进行下面的改造： 

<mirrors>

​     <mirror>

​         <id>nexus</id>

​         <mirrorOf>*</mirrorOf>

​         <url>http://localhost:8081/nexus/content/groups/public</url>

​     </mirror>

</mirros> 

将所有repository的id修改为central，直接覆盖maven超级pom中的morning中央仓库，相当于此时唯一的远程中央仓库变成了我们自己配置的两个仓库。 

然后将url配置全部改为http://central，其实是没意义的一个url，因为此时在需要从远程仓库下载依赖或者插件的时候，会从两个自己配置的central仓库去走，然后看release或者snapshot是否支持，如果支持，那么就会找镜像配置，由于我们的镜像匹配所有请求，所以所有请求都会走镜像，而镜像配置的是私服地址，所以相当于所有的请求都会走私服了。 

我们现在在项目里加入新的一个依赖，比如zookeeper的一个依赖，然后看看理论上来说，应该有的效果，是从私服，私服是从阿里云去下载，私服上会有一份依赖，然后本地仓库里也有一份依赖 

<dependency>

  <groupId>org.apache.kafka</groupId>

  <artifactId>kafka_2.10</artifactId>

  <version>0.10.2.1</version>

</dependency>1627098499

### 19_企业实战场景6：理解nexus权限管理机制&创建部署专用账号

上一讲，我们已经可以让项目从私服拉取依赖了 

我们声明依赖，我们要开发一个功能，需要用到一些依赖，基于nexus私服的机制给我们拉取所有的依赖到本地仓库供我们使用，同时私服上也会缓存一份。接下来我们要干嘛？ 

接下来实际上来说，我们就要使用各种依赖开始写代码，实现各种功能，写单元测试，本地工程跑一跑走一下冒烟测试。比如类似于我们这种大型的项目，每个模块/每个服务，都是一个单独的工程，我们有一个5个人的团队，除了leader不写代码之外，其他4个人每个人都负责一个工程。 

所以我们每个人把自己的代码写好之后，要跟其他就要联调，测试。我们需要将我们的代码部署到私服仓库里面去，让团队中其他人可以从私服里拉取和下载我们写好的代码对应的那个jar包，然后就可以在自己本地调用我们的jar包里面的各种方法和类，按照约定来写代码和测试。 

所以我们现在要学习的就是说，假设我们写好了代码，需要将我们写好的代码打成一个jar包，供其他人去引用和依赖，然后基于我们的代码去开发，此时就涉及到一个问题，如何将我们写好的代码，清理/编译/测试/打包/部署到私服。 

我们需要学习如何将自己的工程打包后发布到私服。 

而且我们没做任何权限认证的事情，那是因为，nexus私服默认就是可以读的，不需要认证，公司局域网内的人都可以去配置之后拉取依赖 

但是如果下一讲要进行部署的话，我们是需要有一个专用的部署账号，通过账号认证，才能部署发布包到nexus私服的 

nexus的权限是典型的RBAC模型，role-based access control。每个用户可以分配多个角色，每个角色分配多个权限，每个权限就是一个具体的功能，比如浏览依赖，部署发布包，等等。 

nexus默认有三个用户： 

admin，管理员账号，密码是admin123

deployment，可以搜索和部署构建，就是普通的开发账号，密码是deployment123（在nexus 3.x最新版本里已经被消除掉了）

anonymous：如果没有给认证信息，就是这个匿名账号，可以下载依赖，查看依赖 

可以在Users页面里添加用户，管理用户，还可以对用户的Role Tree进行角色的分配，nexus有一些预定义的角色，直接可以用。 

也可以对单个仓库创建角色，并给角色分配对仓库的权限，然后就可以将单个仓库的授权角色分配给不同项目的用户，但是说实话，这个很少用。 

接下来要做的一个事情是什么呢？ 

我们光是有admin和匿名账号是不够的，我们需要创建一个专门用来部署的账号，deployment： 

（1）涵盖所有匿名账号的权限，至少可以搜索仓库，下载依赖

（2）对仓库有所有的管理权限，就可以往仓库中去部署发布包1627098499 

### 20_企业实战场景7：将组织机构模块使用专用账号发布至nexus私服

1、部署自己的项目 

我们已经可以让公司内所有项目强制全部走私服了，接下来，就是如果开发好某个模块之后，需要将其发布到私服上去，供其他的项目使用了。 

（1）发布仓库配置 

将项目发布包部署到哪个仓库中，是需要用下面的pom.xml中的配置来设置的： 

<distributionManagement>

​      <repository>

​            <id> nexus-releases</id>

​            <name> Nexus Release Repository</name>

​            <url>http://localhost:8081/nexus/content/repositories/releases/</url>

​      </repository>

​      <snapshotRepository>

​            <id> nexus-snapshots</id>

​            <name> Nexus Snapshot Repository</name>

​            <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>

​      </snapshotRepository>

</distributionManagement> 

（2）部署专用账号的配置 

但是nexus仓库对于普通的匿名用户是只读的，也就是说，只能下载依赖，不能部署发布包，因此如果要能够部署发布包，还需要在settings.xml文件里通过<server>元素配置使用专用的部署用户，来通过认证，进行发布包的部署。 

需要在settings中配置： 

<servers>

​     <server>

​         <id>nexus-releases</id>

​         <username>deployment</username>

​         <password>deployment123</password>

​     </server>

​     <server>

​         <id>nexus-snapshots</id>

​         <username>deployment</username>

​         <password>deployment123</password>

​     </server>

</servers> 

（3）执行Maven deploy命令部署到私服 

clean deploy这个命令，执行maven命令以后会发生什么事情，我们目前先不用管他，因为依赖管理+仓库讲解完之后，我们马上就要讲解maven的命令背后的深入的原理，在干嘛。。。 

你只要知道，我们接下来去执行mvn clean deploy命令，就可以让maven自动给我们编译源代码、运行单元测试、打成jar包、将jar包安装到本地仓库、将Jar包部署到配置的远程私服仓库里面去。 

是这样的，在私服上面，如果上传snapshot版本的jar包呢，在私服上存储的时候，会自动带上一个时间戳。这个主要是因为考虑到在开发过程中会频繁的部署snapshot包，所以会用时间戳来区分一下。但是这个对我们是透明的，如果我们团队里其他哥儿们要用你的这个snapshot jar包，直接还是用你的坐标来声明依赖就可以了，他是不用去考虑这个时间戳的问题的，私服会替你管理好。 

（4）mvn命令的说明 

mvn clean package：清理、编译、测试、打包 

mvn clean install：清理、编译、测试、打包、安装到本地仓库，比如你自己负责了3个工程的开发，互相之间有依赖，那么如果你开发好其中一个工程，需要在另外一个工程中引用它，此时就需要将开发好的工程jar包安装到本地仓库，然后才可以在另外一个工程声明对它的依赖，此时会直接取用本地仓库中的jar包 

mvn clean deploy：清理、编译、测试、打包、安装到本地仓库、部署到远程私服仓库，这个其实就是你负责的工程写好了部分代码，别人需要依赖你的jar包中提供的接口来写代码和测试。此时你需要将snapshot jar包发布到私服的maven-snapshots仓库中。供别人在本地声明对你的依赖和使用。 

2、对于极其少数的jar包手动上传 

有些第三方厂商的jar包，比如特殊数据库厂商的jdbc驱动，或者某个厂商的支付相关的api jar包，是不提供公网下载的，只能从他们那里拿到一个jar包，此时就需要手动将其上传到3rd party仓库里面去。 

新版本里面，其实主要是建议用命令行手动上传的方式，就不用界面上上传的方式了： 

选择3rd party，选择Artifact Upload，上传即可 

​     <server>

​         <id>nexus-3rd-party</id>

​         <username>deployment</username>

​         <password>deployment123</password>

​     </server> 

mvn deploy:deploy-file -DgroupId=com.csource -DartifactId=fastdfs-client-java -Dversion=1.24 -Dpackaging=jar -Dfile=F:\DevelopmentKit\fastdfs_client_v1.24.jar -Durl=http://localhost:8081/repository/3rd-party/ -DrepositoryId=nexus-3rd-party 

### 21_企业实战场景8：为nexus配置自动化的日常管理任务

一般都是需要对nexus设置一些每天自动执行的管理任务的 

### 22_一张图带你彻底看懂maven的生命周期以及plugin执行原理

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\2201.png)

依赖这块已经彻底搞定了，然后就来看看maven的最重要的构建这块 

自动化的依赖管理

自动化的构建管理 

mvn命令 

mvn clean package：打包

mvn clean install：安装到本地

mvn clean deploy：部署到远程私服 

1、一张图带你看懂maven生命周期以及执行原理 

我之前见过很多人，讲解maven这块，一般都是参照那个maven实战那本书的组织结构去讲解的。那本书写的相当不错，但是并不能说就是学习maven的人最好的一个资料。讲解的顺序和组织，语言的表述，有的时候会让人觉得学习的也比较吃力。 

上来就是maven生命周期，我就特别不喜欢那种讲解，我喜欢按照我自己的理解，站在学习人的角度，去考虑大家当前对maven是一种什么样的印象，理解到了什么程度，然后顺着你们当前的思路去讲解 

先讲依赖，按照的工程开发的过程去讲解的，你先用maven搭建一个工程，依赖，开发代码，依赖管理机制，私服，仓库架构，部署到私服 

maven生命周期，就是去解释mvn各种命令背后的原理 

maven的生命周期，就是对传统软件项目构建工作的抽象 

清理、初始化、编译、测试、打包、集成测试、验证、部署、站点生成 

maven有三套完全独立的生命周期，clean，default和site。每套生命周期都可以独立运行，每个生命周期的运行都会包含多个phase，每个phase又是由各种插件的goal来完成的，一个插件的goal可以认为是一个功能。 

这就是maven的生命周期 -> phase（可以理解为阶段） -> 插件的关系，也是maven构建执行的核心原理 

你每次执行一个生命周期，都会依次执行这个生命周期内部的多个phase，每个phase执行时都会执行某个插件的goal完成具体的功能 

2、maven的生命周期以及phase 

clean生命周期包含的phase如下： 

pre-clean

clean

post-clean 

default生命周期包含的phase如下： 

validate：校验这个项目的一些配置信息是否正确

initialize：初始化构建状态，比如设置一些属性，或者创建一些目录

generate-sources：自动生成一些源代码，然后包含在项目代码中一起编译

process-sources：处理源代码，比如做一些占位符的替换

generate-resources：生成资源文件，才是干的时我说的那些事情，主要是去处理各种xml、properties那种配置文件，去做一些配置文件里面占位符的替换

process-resources：将资源文件拷贝到目标目录中，方便后面打包

compile：编译项目的源代码

process-classes：处理编译后的代码文件，比如对java class进行字节码增强

generate-test-sources：自动化生成测试代码

process-test-sources：处理测试代码，比如过滤一些占位符

generate-test-resources：生成测试用的资源文件

process-test-resources：拷贝测试用的资源文件到目标目录中

test-compile：编译测试代码

process-test-classes：对编译后的测试代码进行处理，比如进行字节码增强

test：使用单元测试框架运行测试

prepare-package：在打包之前进行准备工作，比如处理package的版本号

package：将代码进行打包，比如jar包

pre-integration-test：在集成测试之前进行准备工作，比如建立好需要的环境

integration-test：将package部署到一个环境中以运行集成测试

post-integration-test：在集成测试之后执行一些操作，比如清理测试环境

verify：对package进行一些检查来确保质量过关

install：将package安装到本地仓库中，这样开发人员自己在本地就可以使用了

deploy：将package上传到远程仓库中，这样公司内其他开发人员也可以使用了 

site生命周期的phase： 

pre-site

site

post-site

site-deploy 

3、默认的phase和plugin绑定 

但是问题来了，那么我们直接运行mvn clean package的时候，每个phase都是由插件的goal来完成的，phase和plugin绑定关系是？ 

实际上，默认maven就绑定了一些plugin goal到phase上去，比如： 

类似于resources:resources这种格式，说的就是resources这个plugin的resources goal（resources功能，负责处理资源文件） 

process-resources                resources:resources

compile                            compiler:compile

process-test-resources           resources:testResources

test-compile                      compiler:testCompile

test                               surefire:test

package                           jar:jar或者war:war

install                             install:install

deploy                            deploy:deploy 

site生命周期的默认绑定是： 

site                               site:site

site-deploy                       site:deploy 

clean生命周期的默认 

clean                              clean:clean 

到此为止，基本我们就很清楚maven的声明周期、phase和plugin的关系，同时也清楚我们运行类似mvn clean package之类的命令时，到底是在干啥了。。。。。。也知道为啥这个命令一执行，就可以实现清理、打包之类的功能更了。。。 

4、maven的命令行和生命周期 

比如mvn clean package 

clean是指的clean生命周期中的clean phase

package是指的default生命周期中的package phase 

此时就会执行clean生命周期中，在clean phase之前的所有phase和clean phase，pre clean，clean

同时会执行default生命周期中，在package phase之前的所有phase和package phase 

就是这个意思 

mvn clean package

clean生命周期的pre clean，clean两个phase

但是，pre clean phase默认是没有绑定任何一个plugin goal的，所以默认什么也不会干；clean phase默认是绑定了clean:clean，clean plugin的clean goal，所以就会去执行clean插件的clean goal，就会实现一个功能，就是清理target目录下的文件 

process-resources                resources:resources

compile                           compiler:compile

process-test-resources           resources:testResources

test-compile                      compiler:testCompile

test                               surefire:test

package                           jar:jar或者war:war

install                             install:install

deploy                            deploy:deploy

  

mvn clean install

mvn clean deploy

 

mvn dependency:tree

mvn deploy:deploy-file 

就是不执行任何一个生命周期的任何一个phase 

直接执行指定的插件的一个goal 

比如mvn dependency:tree，就是直接执行dependency这个插件的tree这个goal，这个意思就是会自动分析pom.xml里面的依赖声明，递归解析所有的依赖，然后打印出一颗依赖树 

mvn deploy:depoy-file，就是直接执行deploy这个插件的deploy-file这个goal，这个意思就是说将指定目录的jar包，以指定的坐标，部署到指标的maven私服仓库里去，同时使用指定仓库id对应的server的账号和密码。 

下一讲，预告，大家用过maven的人应该都知道，maven里很重要的一块配置，就是各种各样的plugin，然后每个plugin内部都有一些乱七八糟的配置，各种东西，以前看的晕晕乎乎的，但是现在在透彻理解了maven生命周期执行原理之后，你首先就知道了插件是怎么回事。。。

### 23_从此对复杂的plugin配置不再云里雾里：彻底精通插件的配置

生命周期 -> phase -> plugin goal 

mvn clean package，背后的一套原理 

现在，引出来一个问题，就是默认情况下，你什么都不搞的话，其实就少数一些phase绑定了一些maven内置的插件的goal，相当于有大量的phase其实是空置的 

另外一个问题，可能在我们使用maven的过程中，有很多各种各样其他的一些需求，我给大家举个两个最常见的例子： 

（1）比如说，我们打出来的一个jar包，放到生产环境去跑的时候，实际上是需要将所有依赖的jar包都打在这个jar包里面的，这样的话，在实际程序运行的时候，才能找到所有需要的依赖。 

默认maven的功能是不支持的，所以此时就需要用到maven其他的插件，就有很多人或者公司，为了增强maven的功能，基于maven实现各种各样酷炫或者实用的功能，于是基于maven的这一套生命周期+插件的机制，开发了各种各样的插件，然后呢，我们如果要使用别人的插件在我们的项目里实现某些特殊的功能，此时就可以在pom.xml里面配置那个插件，包括最重要的，配置那个插件绑定到哪个phase上去。 

然后呢，在maven的命令执行的时候，不是默认会执行生命周期中的各种phase吗，如果你绑定了某个第三方的插件到phase，此时插件就会执行，然后实现我们想要的功能。 

比如说，我们就可以用assembly的插件，配置在里面，打包的时候，将所有依赖的jar包都放入我们自己工程的jar包里。 

（2）另外一个，就是我们可能需要的是，除了能运行单元测试代码以外，我们还需要在单元测试执行完之后，运行一个其他的插件，计算出单元测试对我们所有代码的一个覆盖率。就是说单元测试，测试了百分之多少的代码。此时也需要使用一个第三方的cobertura插件。

（3）比如我们后面会讲解，开发了一个web工程，需要在本地，在eclipse里面随时很轻易的就启动一个web容器，然后部署这个web工程，而不是用比较重型的eclipse+tomcat的那种方式。运行mvn命令，直接web容器也启动了，web工程也部署好了，接着打开浏览器，访问接口，就可以冒烟测试。这种情况下，也需要使用一个专门的jetty插件。绑定到一个phase，phase结束之后，就直接插件启动一个jetty容器，然后部署工程打好的war包到jetty容器中，对外提供访问。 

就是百度搜索一下，找到你需要的那个功能对应的插件，然后搜一下那个插件配置的范例，接着在pom.xml里面配置那个插件，将插件绑定到某个phase。此时每次你执行mvn命令的时候，插件就可以被执行，然后提供更多的功能。 

来学习如何配置插件，插件配置的语法是什么 

学完了以后，我们以后应该彻底了解两个事情： 

（1）你配置了插件以后，结合生命周期执行原理，了解插件执行的原理是什么

（2）你完全可以看懂眼花缭乱，乱七八糟的那些所谓的插件的配置

（3）我们这一讲不会去练习配置插件，因为后面插件有的你配置，我们上面讲解的那些功能，后面都是需要借助插件去实现的 

1、插件和goal 

plugin 

每个插件都有多个goal，每个goal都是一个具体的功能 

举个例子，dependency插件有十几个goal，可以进行分析项目依赖，列出依赖树，等等 

插件的goal，写法就是plugin:goal，比如dependency:tree 

用mvn dependency:tree，就可以手动执行某个插件的goal，执行某种功能 

<resource>的配置的说明，先简单说一下，后面有一讲，是专门讲解这个资源这一块的，resources:resources，process-resources phase。 

2、将插件的goal绑定到phase上 

maven内置就会绑定一些插件的goal到phase上，上一讲已经说过了 

这里讲解一下，我们如果要使用某个maven插件，如何手动将插件的goal绑定到phase上 

比如将source插件的jar-no-fork goal绑定到verify phase，在完成集成测试之后，就生成源码的jar包，这里可以看到绑定plugin的语法： 

简单说一句，有不少开源的项目，不仅仅会给你提供下载一个可以立即使用的jar包，还会给你提供一个用src结尾的一个jar包，那个jar包里放的就是那个开源项目的源码。 

<build>

​     <plugins>

​         <plugin>

​             <groupId>org.apache.maven.plugins</groupId>

​             <artifactId>maven-source-plugin</artifactId>

​             <version>2.1.1</version>

​             <executions>

​                  <execution>

​                      <id>attach-sources</id>

​                      <phase>verify</phase>

​                      <goals> 

​                           <goal>jar-no-fork</goal>

​                      </goals>

​                  </execution>

​             </executions>

​         </plugin>

​     </plugins>

</build> 

运行mvn verify，就可以看到生成了一个包含源码的jar包 

即使不配置绑定的phase也可以，因为大多数插件都默认内置了要绑定的phase，比如这个插件就内置绑定在package phase。 

3、配置插件 

如果在命令行执行插件，可以用-Dkey=value来进行插件的设置 

比如mvn install -Dmaven.test.skip=true，就是surefire插件在测试的时候提供的参数，设置为true就会跳过测试 

此外也可以在pom.xml中用来配置 

<build>

​     <plugins>

​         <plugin>

​             <groupId>org.apache.maven.plugins</groupId>

​             <artifactId>maven-compiler-plugin</artifactId>

​             <version>2.1</version>

​             <configuration>

​                  <source>1.5</source>

​                  <target>1.5</target>

​             </configuration>

​         </plugin>

​     </plugins>

</build> 

4、找到你需要的插件 

在http://maven.apache.org/plugins/index.html中可以找到所有的插件，里面有文档 

很简单，很接地气的办法，就是一般你可以百度搜索，用maven启动jetty插件，用maven部署tomcat插件 

5、插件解析 

先从本地仓库找插件，没有则从远程仓库找插件 

插件的远程仓库也需要配置，maven默认配置了远程的插件仓库 

<pluginRepositories>

​     <pluginRepository>

​         <id>central</id>

​         <name>Maven Plugin Repository</name>

​         <url>http://repo1.maven.org/maven2</url>

​         <layout>default</layout>

​         <snapshots>

​             <enabled>false</enabled>

​         </snapshots>

​         <releases>

​             <updatePolicy>never</updatePolicy>

​         </releases>

​     </pluginRepository>

</pluginRepositories> 

### 24_完善案例的业务模型：开发权限管理模块

开发一个业务模块：权限管理 

我们说了，要最好是用企业实战场景，和有一点业务背景的案例来驱动，来讲解我们的课程 

类似于maven，git，是属于某一个技术专题 

做一个案例，案例驱动，大家有一点点业务上的感知，学习感觉会更好一点 

这里我说一句题外话，实际上，在培训这一块，迄今为止，我可以说，整个国内，所有线上线下的培训机构，做的所谓的项目，基本都是demo，案例，没有一个是真实的项目的 

包括我自己的之前的一些课程，北风网的spark项目，龙果学院的亿级流量电商详情页系统，号称都是项目课，应该都知道，业务很简单，不叫项目，案例 

但是我们这个架构师的课程，后面的定位是，要用一个完全真实，完全复杂的一个大型的电商系统，业务真实，业务复杂，去做真实项目的实战， 尤其是到了后面，整个项目的讲解和开发过程，完全模拟真实的企业环境 

包括我后面会在阿里云弄四套环境 

平时日常，常有的环境，应该是三套：集成测试环境，QA测试环境，预发布环境 

每次一个阶段结束之后，要将项目部署到线上去，会临时弄一套生产环境，机器特别多，这都是真金白银花钱的，不是开玩笑的 

这个课程，后面的git，都是属于专题技术，案例，demo，驱动 

业务场景做全了，后面才有感觉 

权限管理的一个模块，单独做一个工程出来 

提供一个service接口，比如可以给某个用户授权某个角色，employeeId，roleId，插入一张表中就可以了，解除授权 

insert，delete 

写单元测试的时候，比较方便，插入完了，就删除掉 

CREATE TABLE `oa`.`authorization` (

 `id` INT NOT NULL AUTO_INCREMENT,

 `employee_id` INT NULL,

 `role_id` INT NULL,

 PRIMARY KEY (`id`));

### 25_完善案例的业务模型：开发流程审批模块

CREATE TABLE `oa`.`application` (

 `id` INT NOT NULL AUTO_INCREMENT,

 `employee_id` INT NULL,

 `days` INT NULL,

 PRIMARY KEY (`id`));

### 26_企业实战场景9：基于聚合功能实现多模块统一构建&某学员真实工作问题

我们现在手头有三个模块：组织机构，权限管理，流程审批

假设这三个模块都是一个人在开发，那么势必会有一些场景，比如说是三个模块互相之间有一些依赖，然后呢，其中一个被依赖的基础模块修改了代码，这个时候，我们要干嘛？ 

（1）肯定是要运行一下基础模块的单元测试套件，确保所有单元测试都可以通过

（2）肯定要将修改代码后的模块部署到本地仓库

（3）其他模块要依赖更新版本的snapshot包，

（4）需要运行其他所有依赖这个基础模块的模块的单元测试套件，确保依赖它的其他模块没有任何问题 

组织机构这个模块，修改了代码；权限管理和流程审批都依赖了组织机构，此时就需要将这两个模块都打包运行单元测试套件，检测一下代码没有bug。如果说，我们是有20个模块，依赖了某个基础模块，此时你该怎么办？此时你可能就要依次手工运行20次构建的命令，让20个模块都依次去构建和测试？ 

是不是很麻烦 

所以这里的一个maven的解决方案，聚合功能，就是可以将各个模块，聚合成一个大的模块，给它一个父工程，父模块，那个父模块里面呢配置聚合了哪些子模块 

只要对父模块运行一次构建命令，此时maven会自动对这个父模块下面的所有子模块都运行相应的构建命令，这样就可以保证一键自动化构建所有的模块，不要一个一个依次去构建。 

如果要一次性构建多个模块的工程，那么就需要创建一个父工程，我们可以创建一个os-parent工程，在其pom.xml中加入以下内容： 

<groupId>com.zhss.oa</groupId>

<artifactId>oa-parent</artifactId>

<version>1.0.0-SNAPSHOT</version>

<packaging>pom</packaging>

<name>oa parent project</name> 

<modules>

​     <module>oa-organ</module>

​     <module>oa-auth</module>

​     <module>oa-flow</module>

</modules> 

一般来说会将模块统一放在父工程的目录下，这样就可以用上述形式，否则如果多个工程平级，则要用类似于../oa-organ的相对路径。 

接着对oa-parent运行mvn clean install，此时就会对oa-parent中所有的工程都进行统一的构建 

跟大家说，对于一个大型的工程来说，特别是你去看一些其他开源项目的源码，比如说知名的大数据开源计算框架，spark，它其实就是这种组织方式，源码外面就是一层，展开以后就是各个子模块。 

前几天，有一个学员跟我说的一个问题 

（1）他依赖了其他项目组的一个jar包

（2）他声明了对那个jar包的依赖，但是那个jar包也是有一个parent工程的

（3）本地的maven可以下载到那个jar包的依赖，但是下载不到对应的parent工程的东西

（4）当时他们那个项目组的同事，我觉得对maven也不是很精通，半吊子，仅仅部署了子工程的jar包到私服，但是父工程没有部署到私服

（5）所以那个学员当时说maven报错，下载不到parent一个依赖

### 27_企业实战场景10：基于继承功能将所有模块的依赖版本强制统一

我们通常对同一个系统，需要将各个模块的工程，其中的基础性的、相同的依赖，全部放入一个父工程中，集中式统一约束所有模块的依赖，避免每个模块负责的人胡乱使用依赖版本，比如有的人用spring 3，有的人用spring 4 

如果每个模块是不同的人负责的，结果就可能导致一个问题，有可能： 

（1）有的人可能就是某一天，突然就想升级spring版本到5.0，结果其他人用的都是4.0，最后导致项目集成的时候就出各种问题

（2）比如项目里现在需要用一个组件，activiti，结果每个模块的负责人各自加入自己的依赖，有的人用1.0，有的人用2.0，有的人用3.0，完全没有统一起来 

maven里面提供了一个功能，叫做继承，也就是说，我们可以将一个项目中所有模块通用的一些配置，比如依赖和插件，全部放在一个父工程中，oa-parent，然后所有的子工程，声明一下从父工程中去继承 

如果直接将所有的依赖放入父工程，然后子工程用<parent>元素声明继承，然后此时会强制性继承父工程中所有的依赖和插件。 

但是实际的场景下，不同的工程需要的依赖是不一样的，可能有的工程需要从父工程继承20个依赖和3个插件；有的工程需要从父工程继承10个依赖和2个插件。如果按照上面那种最low最普通的做法，会导致每个子工程都强制性从父工程继承所有的依赖。 

那么这就导致，本来某个子工程只要10个依赖，现在导致每次打包，assembly的大包，可能导致每个工程的jar包打包速度变慢，打完以后的包比较大。这种做法是很不好的，不推荐。 

推荐的一个做法，是在父工程中，使用<depdendencyManagement>和<pluginManagement>两个元素，来声明要被子工程继承的依赖和插件 

有一个好处，就是，如果子工程用<parent>元素声明继承父工程，此时不会强制性继承所有的依赖和插件，子工程需要同时声明，自己要继承哪些依赖和插件，但是只要声明groupId和artifactId即可，不需要声明version版本号，因为version全部放在父工程中统一声明，强制约束所有子工程的相同依赖的版本要一样。 

将oa-organ、oa-auth、oa-flow三个工程的pom.xml中加入以下配置 

<parent>

​     <groupId>com.zhss.oa</groupId>

​     <artifactId>oa-parent</artifactId>

​     <version>1.0.0-SNAPSHOT</version>

</parent> 

然后将所有工程相同的依赖和插件的配置，全部放入父工程中，此时子工程就会继承父工程的依赖和插件，不需要每个工程都重复定义，而且可以在父工程中统一约束所有依赖和插件的版本。 

但是这里有个问题，如果直接将所有依赖和插件直接放入父工程，会导致子工程强制性继承，也许子工程并不需要某些依赖呢？ 

此时最好的做法，就是在父工程中，使用<dependencyManagement>元素和<pluginManagement>元素，来声明所有的依赖和插件。 

此时在子工程中，就可以对自己需要的依赖进行声明，但是不用定义版本号，只要groupId和artifactId即可。这样声明之后才会继承依赖，而且版本由父工程约束。 

提一点，这里很关键的一个知识点，是你要搞清楚，dependencyManagement和pluginManagement的意义在哪儿？一般来说，很多人都没搞清楚用上面说的那两个元素来进行继承，以及直接在parent里面放dependencies和plugins里面放依赖和插件来继承，他们俩的区别在哪儿？ 

如果在父工程中，直接用dependencies和plugins来声明依赖和插件，子工程会强制全部继承；如果用dependencyManagemnent和pluginManagement来声明依赖和插件，默认情况下，子工程什么都不继承，只有当子工程声明了某个依赖或者插件的groupId+artifactId，但是不指定版本时，才会从父工程继承那个依赖。在规范的工程管理中，肯定都是用dependencyManagement和pluginManagemnent的。

### 28_企业实战场景11：对类似spring的依赖集中约束版本号

比如引入spring，或者spring boot，或者spring cloud的多个包，一般就可以统一用properties定义一个版本号，然后下面的依赖全部引用 

如果说，我们默认就保留这种情况，有两个问题 

（1）比如你的公司要统一升级一下spring版本号

（2）这个时候，可能就需要在一个parent的pom.xml里面，一个一个的修改，很麻烦

（3）可能你还会漏掉某个依赖的版本修改和升级，这就会导致都是spring的项目，结果。。不同的子工程的版本不一致，可能在整合使用的时候就会有问题 

所以针对这种情况，一般都会在properties元素里面，统一定义一个版本号，然后在这个类似spring的依赖里面，全部用${}占位符来引用一个版本号，那么每次修改，升级spring版本，就直接修改properties元素中的一个地方就可以了，不会出错 

包括spring boot，spring cloud，都是一样的问题

### 29_企业实战场景12：基于import pom强制约束依赖方的版本号

在企业工作中，有这样的一种场景 

比如说，你开发了一个第三方的一个组件，就是一个不属于任何一个项目的基础的一个组件，比如基于activiti开发了一个流程审批的封装以后的一个组件 

然后呢，其他人如果要用你的这个组件，肯定是在depedencies里面去定义和声明，然后看去依赖你的组件，然后才可以基于你的组件去开发么 

但是有一个问题，假设说他呢依赖了你的组件，然后呢你的组件依赖了activiti 1.3版本，但是有个问题，结果依赖方自己没头没脑的又声明了一个队activiti 1.2的依赖，此时根据依赖调解的原则，肯定是用他自己的旧版本 

比较容易出现依赖的冲突 

所以说，对于公司里一些重量级的组件，一般来说会采取下面的这种方式 

首先自己开发一个工程，那个工程是对外提供服务的；接着再开发一个pom包，这个pom包专门用dependencyManagement来约束依赖方的各个版本号，不要跟自己出现冲突 

然后依赖方，不是直接依赖那个工程自己的包，而是依赖于那个工程的pom类型的包，然后再在dependency里面去声明自己要的依赖，此时版本就被约束了 

<dependencyManagement>

​    <dependencies>

​      <dependency>

​        <groupId>com.zhss.commons</groupId>

​        <artifactId>commons-flow-bom</artifactId>

​        <version>1.2.9</version> 

​        <type>pom</type>

​        <scope>import</scope>

​      </dependency>

​    </dependencies>

   </dependencyManagement> 

可能很多同学都不知道是怎么回事，所以这块儿是这样子，我再把思路给总结一遍 

（1）如果你是公司基础组件的开发人，你的组件依赖了很多重要的第三方的开源框架，然后你为了保证依赖你的组件的人，不会自行导入一些开源框架的过旧的版本，导致跟你的组件出现依赖冲突

（2）你需要为你的组件开发一个类型为pom的工程，后缀名为bom，这个工程，要声明一套dependencyManagement，里面声明好对你的组件的依赖的版本号，还有你的组件使用的重要的第三方开源框架的版本号

（3）然后依赖方在引用你的组件的时候，需要在自己的dependencyManagement中，加一个scope范围为import，类型为pom的这么依赖导入，此时就可以将你的bom包声明的那一套依赖的版本号导入他那里，作为版本的约束

（4）然后依赖方接着在dependencies里面可以声明对你的组件的依赖，此时版本号都不用写，因为已经被你约束了

（5）同时，假设依赖方要自己依赖一个过旧的开源框架的版本，会有提示报警，不让他自行定义过旧版本的框架 

### 30_完善案例的业务模型：开发OA系统的web服务（附完整代码）

写个web工程，依赖3个组件，然后包装3个组件的功能即可 

（1）创建一个web工程，选择maven-archetype-webapp，然后输入坐标 

（2）先删除这个工程，但是不要勾选那个框，否则会彻底删除 

（3）然后到eclipse-workspace目录下面，将oa-web工程拷贝到oa-parent下面去，然后在os-parent的modules中加入oa-web，在oa-web的pom.xml中加入对oa-parent的继承 

（4）删除所有的oa-*工程，但是记住，千万别勾选那个框。然后重新import oa-parent，就会自动导入5个工程进来。s 

（5）声明依赖 

<dependency> 

​      <groupId>junit</groupId> 

​      <artifactId>junit</artifactId> 

​      <version>4.12</version> 

​      <scope>test</scope> 

​    </dependency>  

​    <!-- spring --> 

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-core</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-beans</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-context</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-tx</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-web</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-webmvc</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-jdbc</artifactId> 

​      <version>${spring.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.springframework</groupId> 

​      <artifactId>spring-test</artifactId> 

​      <version>${spring.version}</version> 

​      <scope>test</scope> 

​    </dependency>  

​    <!-- mybatis 包 --> 

​    <dependency> 

​      <groupId>org.mybatis</groupId> 

​      <artifactId>mybatis</artifactId> 

​      <version>3.2.8</version> 

​    </dependency>  

​    <!--mybatis spring 插件 --> 

​    <dependency> 

​      <groupId>org.mybatis</groupId> 

​      <artifactId>mybatis-spring</artifactId> 

​      <version>1.2.2</version> 

​    </dependency>  

​    <!-- mysql连接 --> 

​    <dependency> 

​      <groupId>mysql</groupId> 

​      <artifactId>mysql-connector-java</artifactId> 

​      <version>5.1.34</version> 

​    </dependency>  

​    <!-- 数据源 --> 

​    <dependency> 

​      <groupId>com.alibaba</groupId> 

​      <artifactId>druid</artifactId> 

​      <version>1.0.12</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>org.aspectj</groupId> 

​      <artifactId>aspectjweaver</artifactId> 

​      <version>1.8.4</version> 

​    </dependency>  

​    <!-- log4j --> 

​    <dependency> 

​      <groupId>log4j</groupId> 

​      <artifactId>log4j</artifactId> 

​      <version>1.2.17</version> 

​    </dependency>  

​    <!-- servlet --> 

​    <dependency> 

​      <groupId>javax.servlet</groupId> 

​      <artifactId>servlet-api</artifactId> 

​      <version>3.0-alpha-1</version> 

​    </dependency> 

​    <dependency> 

​      <groupId>javax.servlet</groupId> 

​      <artifactId>jstl</artifactId> 

​      <version>1.2</version> 

​    </dependency>  

​    <!-- json --> 

​    <dependency> 

​      <groupId>org.codehaus.jackson</groupId> 

​      <artifactId>jackson-mapper-asl</artifactId> 

​      <version>1.9.13</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>com.alibaba</groupId> 

​      <artifactId>fastjson</artifactId> 

​      <version>1.2.3</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>com.fasterxml.jackson.core</groupId> 

​      <artifactId>jackson-annotations</artifactId> 

​      <version>${jackson.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>com.fasterxml.jackson.core</groupId> 

​      <artifactId>jackson-core</artifactId> 

​      <version>${jackson.version}</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>com.fasterxml.jackson.core</groupId> 

​      <artifactId>jackson-databind</artifactId> 

​      <version>${jackson.version}</version> 

​    </dependency> 

​    <!-- 文件上传 --> 

​    <dependency> 

​      <groupId>commons-io</groupId> 

​      <artifactId>commons-io</artifactId> 

​      <version>2.4</version> 

​    </dependency>  

​    <dependency> 

​      <groupId>commons-fileupload</groupId> 

​      <artifactId>commons-fileupload</artifactId> 

​      <version>1.2.2</version> 

​    </dependency>       

（6）application-web.xml 

<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"

  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 

  xmlns:context="http://www.springframework.org/schema/context"

  xmlns:aop="http://www.springframework.org/schema/aop" 

  xmlns:tx="http://www.springframework.org/schema/tx"

  xmlns:p="http://www.springframework.org/schema/p"

  xsi:schemaLocation="http://www.springframework.org/schema/beans 

​      http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

​      http://www.springframework.org/schema/context 

​      http://www.springframework.org/schema/context/spring-context-4.0.xsd

​      http://www.springframework.org/schema/aop 

​      http://www.springframework.org/schema/aop/spring-aop-4.0.xsd

​      http://www.springframework.org/schema/tx 

​      http://www.springframework.org/schema/tx/spring-tx-4.0.xsd"> 

  <import resource="classpath*:application-organ.xml" />

</beans>  

（7）spring-servlet.xml 

<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"

  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 

  xmlns:context="http://www.springframework.org/schema/context"

  xmlns:mvc="http://www.springframework.org/schema/mvc"

  xmlns:aop="http://www.springframework.org/schema/aop" 

  xmlns:tx="http://www.springframework.org/schema/tx"

  xmlns:p="http://www.springframework.org/schema/p"

  xsi:schemaLocation="http://www.springframework.org/schema/beans 

​      http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

​      http://www.springframework.org/schema/mvc 

​      http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd 

​      http://www.springframework.org/schema/context 

​      http://www.springframework.org/schema/context/spring-context-4.0.xsd

​      http://www.springframework.org/schema/aop 

​      http://www.springframework.org/schema/aop/spring-aop-4.0.xsd

​      http://www.springframework.org/schema/tx 

​      http://www.springframework.org/schema/tx/spring-tx-4.0.xsd"> 

  <!-- 扫描controller（controller层注入） -->

  <context:component-scan base-package="com.zhss.oa.web.controller"/> 

  <!-- 启动注解支持 --> 

  <mvc:annotation-driven /> 

  <!-- 静态资源 -->

  <mvc:resources location="/WEB-INF/js/" mapping="/js/**"/>

  <mvc:resources location="/WEB-INF/css/" mapping="/css/**"/>

   <mvc:resources location="/WEB-INF/image/" mapping="/image/**"/> 

  <!-- 视图解析器 -->

  <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">

​    <property name="prefix" value="/WEB-INF/jsp/" />

​    <property name="suffix" value=".jsp" />

  </bean>

</beans>  

（8）web.xml 

<!DOCTYPE web-app PUBLIC

 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"

 "http://java.sun.com/dtd/web-app_2_3.dtd" > 

<web-app>

 <display-name>Archetype Created Web Application</display-name> 

   <!-- Spring配置 -->

  <context-param>

​    <param-name>contextConfigLocation</param-name>

​    <param-value>classpath:application-web.xml</param-value>

  </context-param>

  <listener>

​    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>

  </listener> 

  <!-- Spring MVC -->

   <servlet>

​    <servlet-name>springmvc</servlet-name>

​    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

​    <init-param>

​      <param-name>contextConfigLocation</param-name>

​      <param-value>classpath:spring-servlet.xml</param-value>

​    </init-param>

  </servlet>

  <servlet-mapping>

​    <servlet-name>springmvc</servlet-name>

​    <url-pattern>/</url-pattern>

  </servlet-mapping>  

  <welcome-file-list>

​    <welcome-file>index.jsp</welcome-file>

  </welcome-file-list>

</web-app> 

（9）编写controller代码 

（10）单元测试 

@RunWith(SpringJUnit4ClassRunner.class)

@WebAppConfiguration 

@ContextConfiguration(locations = {"classpath*:/applicationContext.xml"}) 

@Resource 

（11）注意点 

看完视频以后，不要直接从这个教案里面去拷贝内容，因为这个教案里的东西不一定是准确的，拷贝到工程里以后可能会修改，大家如果要拷贝一些配置，参考一些代码，直接看随课程发给大家的，因为那个里面都是经过调试，可以运行的代码和配置

### 31_企业实战场景13：基于surefire插件运行单元测试以及输出覆盖率报告

1、surefire插件介绍 

通常我们在开发完成之后，都会编写一些单元测试，但是随着系统日益增长，单元测试会很多很多很多，可能多大数百个，数千个，甚至是数万个。而我们每次如果要将开发好的代码跟别人开发的代码进行持续集成，或者是发布到测试环境联调测试，或者是直接上线，每次都是需要将所有的单元测试都运行一次的，因为这是保障系统质量非常重要的行为。如果所有单元测试都能测试通过，那么这个系统的质量也可以得到保障。 

微服务这一块的架构，集成测试 

假如说有某个哥儿们，写了一段代码，偷懒，嫌弃麻烦，就没运行所有的单元测试，就告诉你说，o了，哥儿们，我要上线了，你觉得你能相信他吗？ 

不能完全依靠对每个工程师的信任，说他每次修改代码肯定会去运行单元测试 

持续集成，自动化集成测试 

在大公司里面，都有持续集成的服务器，jenkins，hudson 

那么实际上，持续集成，说的就是每个工程师当天写完代码之后，都必须将代码合并到一个公共的持续集成的代码分支上面去，一提交，要触发jenkins持续集成服务器，要立即自动化执行一次构建，自动化既有surefire这种插件，去运行所有的单元测试，确保你的代码没有把系统给改坏。 

势必说要用到maven的自动化运行单元测试的插件，surefire 

而我们如果手动进行测试的话，那么是无法想象的，此时就需要用maven来自动化运行单元测试。 

maven中默认内置了surefire插件来运行单元测试，与最新流行的junit单元测试框架整合非常好。一般是在default生命周期的test阶段，会运行surefire插件的test goal，然后执行src/test/java下面的所有单元测试的。 

而surefire插件会根据一定的规则在sre/test/java下面找单元测试类，具体规则如下： 

**/Test*.java

**/*Test.java

**/*TestCase.java 

通常比较流行的是用*Test.java格式类命名单元测试的类 

2、跳过单元测试 

如果某次你的确不想要执行单元测试就打包，那么可以跳过单元测试，但是在实际开发过程中，这是绝对不被允许的。 

用mvn package -DskipTests即可

3、指定想要运行的测试类 

mvn test -Dtest=**Test，直接指定你要运行的类即可 

通常见于开发人员自己刚写了某个单元测试类，然后就想自己运行一下看看效果。可以用逗号隔开，也可以用*匹配多个类。 

4、自定义包含与排除测试类 

<plugin>

 <groupId>org.apache.maven.plugins</groupId>

 <artifactId>maven-surefire-plugin</artifactId>

 <version>2.5</version>

 <configuration>

  <includes>

   <include>**/*Tests.java</include>

  </includes>

  <excludes>

   <exclude>**/*TempTest.java</exclude>

  </excludes>

 </configuration>

</plugin> 

5、测试报告 

单元测试覆盖率是什么意思 

比如说，你总共写了100行代码 

然后你写了3个单元测试方法，结果这3个单元测试的方法会执行你的100行代码中的80行，另外有20行代码是无论运行哪个单元测试都不会执行的 

此时单元测试覆盖率就是80 / 100，80% 

一般单元测试覆盖率，要求的是，两种，方法覆盖率，行覆盖率 

比如你一共写了10个方法，100行代码 

方法覆盖率要达到90%，那么要求就是至少对10个方法中的9个方法都要运行

代码覆盖率要求70%，那么要求就是至少对70行代码都要执行到 

分支覆盖率，如果你的代码中有if else，或者是try catch，相当于是你的一段代码根据不同的情况，可能会有2个分支，分支里面还可以再嵌套if else，再分出来2个分支 

if 

 if else

else

if else 

2 * 2 = 4 

你的单元测试可以覆盖几个分支，1/4，25%；2 / 4，50%分支覆盖率 

surefire插件默认就会在target/surefire-reports目录下生成错误报告，就是如果有单元测试运行失败的话，就会有。 

同时可以引入cobertura插件，可以看到测试覆盖率的报告 

<plugin> 

​        <groupId>org.codehaus.mojo</groupId> 

​        <artifactId>cobertura-maven-plugin</artifactId> 

​        <version>2.5.1</version> 

​      </plugin> 

为什么不配置executions去指定这个插件的goal绑定到哪儿？

为什么不配置configuration特殊的参数 

第一个问题，其实每个插件你只要定义它的坐标，默认情况下，它自己内部就带了要把哪些goal绑定到哪个phase上去 

第二个问题，默认如果没有特殊的要求，用默认的参数就够了

### 32_企业实战场景14：基于jetty插件在本地启动web服务进行功能测试

我们上一讲，已经说了，单元测试这一块的内容 

一般来说，我们在公司里面开发，都是一个常规的路径，写代码，写单元测试，运行单元测试，看一看单测覆盖率，覆盖率不达标，得继续写单元测试，覆盖大部分的代码 

但是对于这个web工程来说，除了在本地单元测试以外，你还需要在本地启动web服务，然后调用一下web接口，简单的测试一下，做一个简单的冒烟测试，看看基本的功能是否正常 

冒烟测试，微软那边传出来，开发工程师，不能完全依赖于测试工程师去帮你检测代码的质量，至少说你在把代码交付给测试工程师去检测之前，你需要用抽一根烟的时间，简单对你的代码核心和基础的功能，跑一下，看看，基本可以跑通。一般来说，测试工程师，吐槽最多的就是，开发工程师自己看都没看，写完代码就直接给我提测了。 

加入jetty插件的配置 

<plugin>

​     <groupId>org.mortbay.jetty</groupId>

​     <artifactId>jetty-maven-plugin</artifactId>

​     <version>7.1.6.v20100715</version>

​     <configuration>

​         <scanIntervalSeconds>10</scanIntervalSeconds>

​         <webAppConfig>

​             <contextPath>/test</contextPath>

​         </webAppConfig>

​     </configuration>

</plugin> 

scanIntervalSeconds，指的是自动扫描项目代码变更的时间 

contextPath，就是部署之后的contextPath 

jetty插件的groupId不是默认的org.apache.maven.plugins，因此如果要执行该插件，需要在settings.xml中进行配置： 

<pluginGroups>

​     <pluginGroup>org.mortbay.jetty</pluginGroup>

</pluginGroups> 

接着mvn jetty:run，执行jetty插件的run goal，会用jetty容器启动web应用，默认绑定8080端口 

用mvn jetty:run -Djetty.port=8081可以修改端口号

settings.xml文件里，有一个东西叫做pluginGroups 

就是我们一般如果配置了插件以后，然后如果我们就直接在命令行运行jetty:run，cobertura:cobertura命令，其实用的是插件的缩写，jetty插件，cobertura插件。但是如果要将插件的缩写和插件自己的groupId对应起来的话，有的时候还是需要你去手动做一下配置的，不然在某些环境下可能直接运行类似jetty:run的命令可能会有问题。 

但是有些插件，比如说dependency:tree，他的groupId，是org.maven.plugins，这个是默认的maven内核中的插件，所以不需要额外配置，但是对于我们上面说的两个插件，最好是配置一下。

### 33_企业实战场景15：基于cargo对web服务进行自动化部署

想象这么一件事情，我们写好了代码，做了单元测试，覆盖率，本地冒烟测试也跑了，接下来要干嘛？ 

接下来一般情况下，就是要部署到集成测试环境，跟其他的同学部署的工程进行集成测试，或者如果没有这个环节，就是部署到QA测试环境，让测试工程师来进行测试。 

这里就涉及到了web工程的部署 

一般情况下，在很久很久以前，10年以前，很low的年代，怎么部署，弄一个测试服务器，上面装一个tomcat，然后呢先停止tomcat，然后把原来的那个项目的war包删除了，然后用eclipse的一个插件，export war包，导出来包含了所有依赖的这么一个war包，然后放到测试服务器的tomcat中，接着重新启动tomcat。 

手工的操作，而且还要登录测试服务器，停止和启动tomcat，很麻烦，很容易出错 

maven，自动化的部署 

cargo插件，就是专门用来进行将本地的web工程打包成一个war包，然后呢扔到远程的服务器上的Tomcat中去，然后就是给你自动化的部署 

在搞定了整个系统的单元测试，web测试之后，就需要将整个系统完成构建，打出一个完整的war包，然后部署到web容器里去了，此时就要用cargo插件来进行自动化的web部署 

<plugin>

​     <groupId>org.codehaus.cargo</groupId>

​     <artifactId>cargo-maven2-plugin</artifactId>

​     <version>1.0</version>

​     <configuration>

​         <container>

​             <containerId>tomcat6x</containerId>

​             <type>remote</type>

​         </container>

​         <configuration>

​             <type>runtime</type>

​             <properties>

​                  <cargo.remote.username>admin</cargo.remote.username>

​                  <cargo.remote.password>admin</cargo.remote.password>

​              <cargo.tomcat.manager.url>http://localhost:8080/manager</cargo.tomcat.manager.url>

​                  <cargo.servlet.port>8080</cargo.servlet.port>

​             </properties>

​         </configuration>

​     </configuration>

</plugin> 

同样需要在settings.xml中加入pluginGroup：org.codehaus.cargo 

然后执行mvn cargo:deploy即可部署 

（1）先在本地安装一个tomcat 6

（2）给tomcat 6设置一个管理员，在conf/tomcat-users.xml， <user username="admin" password="admin" roles="manager"/>

（3）修改tomcat的conf/server.xml，里面在8080那个地方，加入两个配置，URIEncoding="UTF-8" useBodyEncodingForURI="true"

（4）启动tomcat 

（5）在项目的pom.xml里加入cargo插件的配置，直接从我给的代码里面去拷贝

（6）在settings.xml里加入pluginGroup，org.codehaus.cargo

（7）解决中文编码的问题，先在web.xml里加入一个字符集编码的过滤器，直接从代码里面拷贝

（8）运行mvn clean package命令，打包，必须先有一个war包

（9）到这一步为止，就可以部署了，直接cargo:deploy，第一次部署，在http://localhost:8080/oa-web/，跟上接口，就可以去访问了

（10）以后重复部署，cargo:redeploy，自动会先卸载，然后再重新部署 

注意事项1：在执行cargo命令的时候，切记，不要打开tomcat的任何配置问加你，因为也许会因为文件打开占用，导致一些不必要的bug 

注意事项2：如果在实验过程中，遇到一些奇怪的问题，可以考虑在tomcat的webapps下面，删除oa-web目录和oa-web.war，接着重新先关闭tomcat，再启动tomcat，然后再次把你的工程package打包，然后cargo:deploy部署

### 34_企业实战场景16：基于资源过滤+profile功能自动适配各个发布环境 

我可以这么告诉大家，我在见过的所有的maven视频课程里，从没见过任何一个人讲解这块内容，profile适配各个发布环境，但是实际上，在公司里工作的时候，如果你的项目没有用profile去适配各个发布环境，那我只能说，你的项目弄的有点low 

因为如果你不这么做的话，就可能导致你对每个环境的发布，都要手工去修改各种配置文件 

1、公司里的多个环境 

一般我们的项目都有多个环境，常见的是这么几个环境 

dev：即本地的开发和测试环境，这个就是指的是我们自己本地的笔记本电脑，在上面可以进行开发、单元测试、冒烟测试。比如说你的local本地环境需要有一些基础性的依赖设施，比如说数据库，比如说MQ，比如说redis。这些东西，mysql、rabbit mq、redis，都是部署在公司的一套公共的一个dev环境，有一套服务器，上面会部署各种项目组成员在本地开发，需要的各种依赖设施。保证你在本地开发的时候，指定对应的地址，都是可以连通，笔记本电脑连通的。 

beta：beta，一般称作内部测试环境，也就是不对外。集成测试/联调测试的环境，一般自己感觉开发好了之后，就需要将自己的系统部署到一个集成测试环境，有的公司也叫做联调测试环境，说白了，就是一个项目都是多个研发人员搞的，每个人弄好了自己的部分，都要发布到一个环境，大家在上面测一测整个系统多个服务能不能串起来跑的通，至少别报错把。这一块主要是确保主流程要跑通，不要报错。 

test：QA测试环境，正经项目，都会有专门的测试人员，如果没有的话，那研发人员只能自己去充当QA角色了。就是需要将通过集成测试的代码，部署到QA测试环境，然后由QA人员进行非常充分而且完善的测试，验证功能，性能，等各个方面都没有问题 

staging：预发布环境，通常这个环境会跟生产环境保持基本一致，部署到上面之后，就会使用部分真实的流量或者数据，来让系统运行。比如对外提供服务的网站，app之类的，可以通过流量拷贝的方式，拷贝一小部分流量过来，在staging环境让开发好的系统跑一跑。后者也可以拷贝部分真实的线上数据库的数据下来，跑一跑。然后这个环节QA还是会介入，再次验证一下看系统是否运行正常。同时这个环节，有一个验收的作用，项目如果有产品经理，此时会在这个环节看一下是否符合他的产品预期。 

prod：生产环境，最终系统部署到线上生产环境中，完成上线 

每个环境，都是完全隔离开来的，都有自己的数据库、mq、缓存等等各种各样的依赖，比如数据库把，使用的就肯定也是不同的数据库 

dev环境下，通常是用的开发人员自己本地安装的一个数据库，或者是用公司测试环境中专门用于开发的一个公共测试数据库；dev环境下，有一个专门的集成测试数据库；test环境下，有一个专门的QA测试数据库；staging环境下，有一个专门的预发布环境数据库；prod环境下，就是线上的生产环境数据库 

每个环境的数据库的各种配置都是不一样的 

如果我们按照现在这种模式去做项目，是很low，无法直视。相当不靠谱，因为这个是依靠人力去大量的重复的反复的修改，很可能哪一次就弄错了配置。比如说，你本来应该上test环境，数据库+mq+缓存，都是应该test环境的地址；结果你不小心忘了配置，就导致带着beta环境的地址上了test环境，结果导致系统各种报错，耗费1天的时间反复排查。 

还有更严重的情况，比如说带着staging环境的地址，就直接上prod环境，开除 

因此我们的项目，需要一种能力，可以在不同的环境发布的时候，各种资源的配置自动适配各个环境，而不是每次发布一个环境之前，手动去修改那个配置 

我们需要在项目里，给每一套环境，都配置好一套预先定好的地址，而不是每次部署的时候去修改一个文件，接着在每次往一个环境去部署的时候，可以手动指定一个参数，那么那个参数会指定用哪个环境下的一套配置 

那么可能有的同学会说，是，不用每次部署都修改几十个配置了，但是还是要指定一个环境参数，会不会指定错呢？ 

也有这种可能，但是问题在于说，已经概率小的多了，本来你每次部署都要修改几十个配置，现在的话就是指定一个环境参数就可以了 

另外一个，还可以跟大家普及一下，大公司里面，绝对都是对工程开发流程与发布，等等，一整套东西，都是公司自己研发的软件系统。比如说，可能你在各个环境部署的时候，就是只要保证代码仓库里是正确的代码，接着在界面上直接点击要部署到哪个环境就可以了，此时就会将代码部署到那个环境，同时采用正确的环境参数，激活一套配置 

这个的好处，在于说，如果你本来是要上线，部署prod环境；不小心在页面上点错了，点了部署staging环境，也没关系，他其实就会给你部署到staging环境，而且是用staging环境的一套配置，跟prod环境没关系 

2、基于资源过滤+profile的方式适配各个环境 

比如src/main/resources下，有一个jdbc.properties配置文件 

在local环境下，应该是这样的：

database.jdbc.driverClass=com.mysql.jdbc.Driver

database.jdbc.connectionURL=jdbc:mysql://192.168.31.110:3306/oa_local

database.jdbc.username=local

database.jdbc.password=local 

在dev环境下，应该是这样的： 

database.jdbc.driverClass=com.mysql.jdbc.Driver

database.jdbc.connectionURL=jdbc:mysql://192.168.31.110:3306/oa_dev

database.jdbc.username=dev

database.jdbc.password=dev 

此时，首先，我们需要将配置对应的值从配置文件里抽取出来，用占位符替代，然后实际的值放到profile里去 

database.jdbc.driverClass=${database.jdbc.driverClass}

database.jdbc.connectionURL=${database.jdbc.connectionURL}

database.jdbc.username=${database.jdbc.username}

database.jdbc.password=${database.jdbc.password} 

然后在pom.xml里加入各个环境对应的profile配置 

<profiles>

<profile>

<id>local</id>

<properties>

<database.jdbc.driverClass>com.mysql.jdbc.Driver</database.jdbc.driverClass>

<database.jdbc.connectionURL>jdbc:mysql://192.168.31.110:3306/oa_local</database.jdbc.connectionURL>

<database.jdbc.username>local</database.jdbc.username>

<database.jdbc.password>local</database.jdbc.password>

</properties>

</profile>

<profile>

<id>dev</id>

<properties>

<database.jdbc.driverClass>com.mysql.jdbc.Driver</database.jdbc.driverClass>

<database.jdbc.connectionURL>jdbc:mysql://192.168.31.110:3306/oa_dev</database.jdbc.connectionURL>

<database.jdbc.username>dev</database.jdbc.username>

<database.jdbc.password>dev</database.jdbc.password>

</properties>

</profile>

</profiles>

这个时候大家会发现，太好了，配置文件里的值都是占位符，每个环境的实际值都到了对应的profile里去了 

但是此时就是，如何让项目在发布到不同环境的时候激活profile，并且将实际值替换到占位符里去呢？ 

为src/main/resources目录开启资源过滤功能，让maven resources插件在处理资源的时候自动去解析里面的占位符，然后找到对应profile里的实际值来进行替换 

<resources>

<resource>

<directory>${project.basedir}/src/main/resources</directory>

<filtering>true</filtering>

</resource>

</resources>

<testResources>

<testResource>

<directory>${project.basedir}/src/test/resources</directory>

<filtering>true</filtering>

</testResource>

</testResources> 

src/main/profiles 

<resources>

​          <resource>

​            <directory>src/main/profiles/local</directory>

​          </resource>

​        </resources> 

mvn clean package -Pdev，-P就是说激活dev profile 

查看target/classes下面的资源文件，全都替换为了dev profile中的实际值 

profile有很多种激活的方式，但是常用的其实就是-P这一种激活方式即可，因为都是部署的时候用不同的-P来激活 

3、默认激活 

我们一般会设置默认激活一个profile 

在profile配置里加一个： 

<activation>

<activeByDefault>true</activeByDefault>

</activation> 

然后在各个环境部署的时候，肯定会用哪个环境的-P来激活对应的profile配置进行占位符替换 

而且还可以用mvn help:active-profiles查看当前激活的是哪个profile 

4、不同地方配置profile 

profile实际上是可以在不同的地方配置的 

比如说在settings.xml里，我们也通过配置profile以及激活一些profile配置了maven私服仓库地址，在maven的超级pom里，也有一些profile激活 

5、实验步骤 

（1）将jdbc.properties中的属性值都修改为占位符形式 

（2）在pom.xml里面，放入profiles，各个环境对应一个profile，在profile里面定义properties 

（3）在<build>的<resource>里面，开启src/main/resources目录的资源过滤 

（4）在打包的时候，给一个环境参数，激活你要部署的那个环境的profile，同时maven会对src/main/resources下面的文件进行资源过滤，看有没有占位符，如果有，就用profile下面的properties实际属性值去替换 

（5）打包实验，给不同的环境参数 

mvn clean process-resources，不加任何参数，会发现，jdbc.properties里面都替换成了dev profile默认激活的值 

mvn clean process-resources -Pbeta，会激活beta profile，此时，直接到eclipse的eclipse-workspace里面去找，target/classes下面，有处理好占位符的资源文件，此时可以看到，jdbc.properties里面的值都替换成了beta profile下的 

（6）进一步，在大型的工程里面，配置文件特别多，你不可能把所有的配置都放profiles里面，那个有点很乱，一般是怎么做呢？src/main/resources下面直接就不放东西了，实际上是在profile里面定义一个单独属于自己的环境的一套配置文件对应的目录，然后在处理资源文件的时候，会把激活的profile对应的一套目录里的资源文件拷贝到src/main/resources下面去，给工程打包使用 

再建多个目录： 

src/main/profiles/dev

src/main/profiles/beta

src/main/profiles/test

src/main/profiles/staging

src/main/profiles/prod 

每个目录下，都有自己的一套完整的配置文件

因为实际上来说，对于不同的环境，不仅仅只是一些变量值等等，可能对于一些框架本身的设置都不太一样，测试环境，可能就会把一些参数调整的低一些，生产环境就会很高。测试环境下，有些框架就不需要配置，生产环境下需要配置。 

给每个profile配置一下这个要拷贝到打包目录里面去的resource即可： 

<build>

​                  <resources>

​                      <resource>

​                          <directory>src/main/profiles/dev</directory>

​                          <includes>

​                              <include>**/*.xml</include>

​                              <include>**/*.properties</include>

​                          </includes>

​                          <filtering>true</filtering>

​                     </resource>

​                  </resources>

​             </build> 

再次用mvn clean process-resources -Pdev激活各个profile，看一下classes里面是什么？ 

我可以给大家说一个实际的场景： 

第一步，oa-organ，oa-auth，oa-flow，三个模块的负责人，此时用的snapshot版本，mvn clean deploy -Pbeta，此时就会用beta环境的东西去打包和部署到私服，给别人去用 

第二步，oa-web，要部署beta的tomcat，此时依赖的还是各个工程的snapshot版本，此时各个工程的最新的snapshot版本对应的Jar包里面就都是beta环境的配置文件了 

第三步，oa-web，cargo发布到哪个环境的配置，也是可以用profiles下面的properties来进行占位符替换的，每个profile定义一个properties，里面包含自己的环境对应的tomcat地址，然后在cargo插件的配置里面，用占位符来引用即可。 

第四步，oa-web，打一个war包，此时也是mvn clean package -Pbeta，用beta环境的配置打成war包，而且此时用的都是另外三个模块的beta环境下的jar包，打在他自己的war包里面 

第五步，oa-web，用cargo，mvn cargo:redeploy -Pbeta，此时就可以进行beta环境的部署 

第六步，大家在各个环境如法炮制全都测试完了，此时呢就会上线。各个依赖模块，用mvn clean deploy -Pprod，去打一个prod环境的jar包，部署到私服；oa-web，打出来一个war包，此时打包的时候，用的都是各个依赖最新的prod环境的jar包，接着就使用mvn cargo:redeploy -Pprod，此时就会发布到prod环境对应的tomcat地址上去。   

在各个公司，可能用法不太一样，但是maven profile肯定是要用的，因为无论你怎么部署，肯定是涉及多个环境的，不同环境的资源文件放一个profile对应的目录下，然后各种部署或者打包的时候，使用对应的profile的参数来激活即可。

### 35_一张图给你讲清楚版本管理和版本控制的区别以及关系 

版本管理：通常指的是maven中的version，项目的版本管理，随着整个功能的开发，bug的修复，或者大的架构升级，通常来说，都会增加版本号 

版本控制：代码版本控制，git中的多次提交，每次往git代码仓库提交一次代码，这个代码的版本就变更了一次，而git会自动记录下来每次代码版本的变更。因为每次提交代码，相当就是代码的版本就变更了一次。 

版本管理，其实是管理你整个项目的版本，比如现在是1.0版本，接着是1.1版本，再接着是1.2版本，以此类推 

版本控制，指的是代码的版本，用的是svn或者git之类的代码仓库来搞的，相当于代码仓库可以记录下来每次你的代码提交和代码变更，也就记录下来了你的代码的各种版本的变化，大概就是这个意思 

但是他们俩之间通常是有联系的，因为一般来说，每个项目版本，都对应着一个代码仓库的分支。比如说项目的1.0-SNAPSHOT就对应着git中的一个1.0-SNAPSHOT代码分支，然后这个版本的代码各种提交，都是在这个分支上进行的。同时git作为代码仓库，也记录下来了在这个分支上你每次提交的代码版本。 

用一张图给你解释清楚

![](C:\Users\zy199005\Desktop\中华石杉\images\java\01\3501.png)

### 36_企业实战场景17：互联网公司最佳实践之如何进行版本管理 

在maven中的版本如何管理 

1、版本到底是什么 

版本分为两种，一种是快照（snapshot）版本，一种是发布（release）版本 

快照版本就是版本号后面加上SNAPSHOT，比如1.0.0-SNAPSHOT 

发布版本就是版本号上不加SNAPSHOT的，比如1.0.0 

而且版本通常而言是三位的，特别是我们自己公司里的项目，就是比如1.0.0 

1.0.0中的第一个1指的是一个重大版本，比如已经基本成型的一个系统架构 

也有不少项目一开始是0.0.1这样的版本，这个指的就是这个系统刚开始起步，甚至都没能形成一个完整的东西出来，只是少数核心功能也出来了 

1.0.0中的第二个0指的是一个次要版本，比如1.1.0，那么就是加了一些新的功能或者开发了一些新的模块，或者做了一些较大的代码重构，技术升级改造 

1.0.0中的第三个0指的是日常迭代的一个增量版本，比如1.1.1，一般对应着修复了一个bug，或者对某些代码做了轻微的优化或者调整  

形象一些： 

假设，咱们现在有一个电商系统，一开始比较low，就是spring boot快速做出来的，1.0.0-SNAPSHOT，内部开发和测试，1.0.0。一开始1.0.0这个版本包含了商城首页、购物车、订单系统、物流系统，就这么几个模块。 

接下来开始对第一个版本进行迭代，比如说这次要新增一个用户评论功能，此时就会进入一个版本，叫做1.1.0-SNAPSHOT，在内部先开发和测试。接着发布1.1.0版本，此时这个版本就新增了一个用户评论的模块。 

不巧，突然发现某天，购物车这块有个bug，就是说，5分钟没有支付，就莫名奇妙session失效，导致购物车里的东西没了。但是产品需求是说购物车里的东西默认保留7天。修复这个bug，1.1.1-SNAPSHOT，这个bug的修复在内部先开发和测试，让产品经理来验收。最后发布，此时版本变成了1.1.1。此时代码就是包含了一个bug的修复。 

后面就新增了抽奖模块，1.2.0。新增了个性化推荐功能，1.3.0，修复了几个bug，1.3.5，版本。 

接着，咱们发现，这个现有系统架构有点问题，主要是多人写作开发，一个30人的团队，全部对一块代码在各种修改，很恶心。所以开始改造和升级到微服务架构。 

整个系统架构出现了重大的变化，就开始进入2.0.0-SNAPSHOT的开发，这个过程，比如说先把核心的几个模块改造了spring cloud的微服务化。2.0.0代码发布。 

接着，开始陆续对几个非核心的模块进行微服务的改造，比如评论模块，单拉出来作为一个服务，此时进入2.1.0版本。修复了几个bug，变成了2.1.9版本。 

然后又开始，用户量变成了1000万，现有架构无法支撑高并发的访问，高可用的保障。此时又要升级系统架构，高并发高可用的架构，基于各种外部设施以及系统架构的改造来做。此时版本会进入3.0.0-SNAPSHOT版本。3.0.0，3.1.0，3.5.15。 

上面的是互联网公司里，比较通行的一个版本迭代演进的方式，而且也是比较简单的一种。 

如果有的项目，你要拆分的更细，那么可能还可以有4位，1.2.3.15。具体根据你的项目来定。 

此外maven的标准版本规范里，还包含了一个1.0.0-beta-1，最后一个1代表的是增量版本的一个里程碑，就是在进行某个bug修复，或者功能调整的时候，是分为多个步骤，也就是多个里程碑的，那么此时可能就会对应了1.1.1-beta-1，1.1.1-beta-2，1.1.1-beta-3，这样好几个里程碑版本，每个里程碑版本代表完成了一个小阶段 

这里的beta代表的是公开测试版，就是对外提供试用的 

也有的项目用的时alpha来替代beta，alpha也可以理解为实验版本，也就是内部测试版本，就是给自己公司内部用用的 

也有rc版本，就是预发布版本，基本就比较稳定了，但是还不是最终发布版，可以尝试下载使用了 

带你看看spring boot的版本 

并不是这几位的数字都需要使用的，一般我们自己的项目里，用3位就可以了，通常我们的经验是这样的： 

除非进行大的架构升级改造，才会增加大版本，这个很少很少，一般几年才一次 

次要版本，一般就是对应各种需求，比如一个持续几周，或者一个月或者了几个月的大需求，对应一个次要版本

增量版本，一般就是一个大需求中的多个小需求，每个小需求可以给一个增量版本，或者是紧急fix了一个bug，那么就增加一个增量版本 

2、版本是如何变更的 

通常来说，在开发的时候，版本都是SNAPSHOT版本，也就是快照版本，比如说1.0.0-SNAPSHOT此时代码还在不断开发和修改，或者进行测试中，还没有完成所有的测试 

这个时候的版本通常用于多个系统间协调开发，给其他系统去引用你开发的SNAPSHOT版本，让人家可以测试或者联调 

然后在一个SNAPSHOT版本开发完之后，同时通过了完善的测试，版本就会升级到release版本，也就是不带snapshot后缀的版本，比如说1.0.0，此时就是让依赖方也改为这个发布版本，然后就可以上线了 

比如1.0.0-SNAPSHOT在开发中，接着正式发布了1.0.0；然后又开始了1.1.0-SNAPSHOT的开发了，接着正式发布了1.1.0；然后又开始了1.2.0-SNAPSHOT的开发了，接着正式发布了1.2.0，以此类推 

如果要将snapshot发布发布为release版本，至少需要满足几个条件： 

（1）所有的单元测试全部通过

（2）pom.xml中没有任何snapshot版本的依赖

（3）pom.xml中没有任何snapshot版本的插件

（4）这个版本的代码已经全部提交到对应的git分支上了，同时一定从分支merge到了master主干分支上去，并且给master分支打上了一个tag，这个tag就是对主干分支这一时刻代码的一个标签，这样任何时候都可以回退到主干分支的任何一个tag对应的版本的代码上去 

此时一个release版本就完成了，然后就继续升级到下一个版本的snapshot版本上去，继续进行开发和测试 

3、版本与主干、分支以及标签 

其实一般就是一个版本对应一个分支，各种开发+测试 

搞定之后就会merge到主干 

然后就会给这个时候的主干打一个tag作为主干在这个版本的代码，以后任何时候都可以使用主干的某个tag，也就是某个时刻的某个版本的代码

### 37_企业实战场景18：站在架构师视角为项目创建统一的工程骨架 

1、archetype模板的介绍 

artchetype其实就是个maven项目模板 

比如我们之前在eclipse里面用m2eclipse插件自动创建的maven工程，就是用的maven-archetype-quickstart模板，就是给我们创建好最基础的工程结构和pom信息 

（1）公司角度 

在公司里，如果你是个架构师，那么需要站在架构师的视角，为公司创建一个统一的archetype模板，里面包含了一些必要的依赖和插件等等信息 

举个例子吧，比如你公司用的一整套技术栈，可能就是spring boot+spring cloud+mysql+redis+zookeeper+rabbitmq+elasticsearch，包括一些zookeeper基础工具类，redis的基础工具类，mysql的一套公司的DAO框架，elasticseearch访问的基础工具类。同时还有一些maven私服部署的配置，插件的配置，那么你就可以给你的公司创建一个archetype，里面包含这些东西 

你可以作为一个架构师，给你的公司，定制一个通用的项目骨架，里面包含了上面说的所有东西。任何一个项目，如果要启动，都不需要任何人手动重新去查阅资料，然后复制粘贴，构建工程，配置插件，光是搭建一个项目骨架，就要好几天的时间。 

更痛苦的事情，如果类似的技术架构，不同的人用不同的方式去写一些基础工具类，版本去搭建，会导致各个项目组之间的成员，进行项目的接手和维护会比较复杂。耗时较长，比较麻烦。 

archetype项目模板，直接创建一个下项目工程出来，基于此直接开发。公司里面各个项目，很多框架版本，框架整合，基础代码，代码规范和约定，都差不多。 

可能你们已经做好了一个项目了，maven这一套东西都很稳定了，此时就可以完全将稳定不变，各个项目可以他通用的东西抽取出来，做成一个archetype模板 

然后下次你们如果要新启动一个项目，直接用archetype模板创建出来工程即可，比如就创建一个parent父工程出来，然后那个项目中的每个服务就继承自自己项目的parent工程即可，很快速 

而且各种技术的版本都是整合好的，很方便使用，不用自己花时间去想用什么版本，也许还要解决依赖冲突问题什么的，都不用管了，直接上手用就可以，很方便，还便于公司内部统一技术栈 

然后每个项目自己可以在基础之上，修改自己的pom配置，或者加入其它依赖 

（2）项目角度 

可能比如说，类似我们这个oa的项目，之前几个模块，其实一些框架的整合啊什么的，都是差不多的，资源配置文件，也是差不多的，此时，我们刚才搭建工程，还是手工方式去搭建，然后复制黏贴，各种修改，其实是比较麻烦的。而且可能出错。 

对于一个项目来说，可能会不断拆分多个模块对应的工程，也可能会有不同的服务。此时架构师完全可以针对这个项目，制定一个项目的骨架，也就是一个archetype，然后呢，项目组内的成员，每次要新构建一个工程，直接用archetype骨架来生成即可。很方便。 

（3）练习 

用第二种情况来举例子，其实大家学会了之后，就完全可以自己去弄了 

2、常用的maven archetype 

如果只是一个模块，或者不需要web容器的支持，那就是maven-archetype-quickstart即可 

如果是一个web工程，那可以用maven-archetype-webapp 

3、自己写一个archetype 

（1）用eclipse提供的向导，常见一个maven工程，选择的archetype是maven-archetype-archetype，就是用来创建archetype的一个模板 

（2）熟悉archetype自己的pom.xml 

创建一个maven工程，将其改造为archetype需要的格式 

archetype自己也需要一个pom.xml，用来定位自己的坐标 

<groupId>com.zhss.maven.archetypes</groupId>

<artifactId>maven-archetype-parent</artifactId>

<version>1.0.0</version> 

（3）要生成的项目的pom.xml 

接着编写src/main/resources/archetype-resources/pom.xml，就是基于这个archetype生成的项目的pom.xml，里面要注意的是要用到一些占位符，因为生成项目的时候，groupId和artifactId之类的都是用户输入的，这个就是最终生成的项目的pom.xml文件。里面可以放入任何你需要的依赖和插件。 

<groupId>${groupId}</groupId>

<artifactId>${artifactId}</artifactId>

<version>${version}</version>

<name>${artifactId}</name> 

下面放入依赖、插件以及部署等通用的信息 

（2）定义archetype的元数据 

接着要编写src/main/resources/META-INF/maven/archetype-metadata.xml 

<?xml version=”1.0” encoding=”UTF-8”?>

<archetype-descriptor name=”parent”>

<fileSets>

<fileSet filtered=”true” packaged=”true”>

<directory>src/main/java</directory>

<includes>

<include>**/*.java</include>

</includes>

</fileSet>

<fileSet filtered=”true” packaged=”true”>

<directory>src/test/java</directory>

<includes>

<include>**/*.java</include>

</includes>

</fileSet>

<fileSet filtered=”true” packaged=”true”>

<directory>src/main/resources</directory>

<includes>

<include>**/*.properties</include>

</includes>

</fileSet>

</fileSets>

<requiredProperties>

<requiredProperty key=”port” />

<requiredProperty key=”groupId”>

<defaultValue>com.zhss</defaultValue>

</requiredProperty>

</requiredProperties>

</archetype-descriptor> 

这个文件两个作用，第一个作用是定义将这个archetype中的哪些java代码和测试代码以及资源文件，都包含到创建好的项目中去；第二个作用是定义创建项目的时候需要输入的参数是什么

（3）编写要给新项目的java代码、测试代码以及资源包和配置文件s 

src/main/java之类的，对应的目录是src/main/resources/archetype-resources/src/main/java子目录中的代码，在这里可以写你要预先给好的代码 

src/test/java，对应的目录是src/main/resources/archetype-resources/src/test/java子目录中的代码，就是你预先给好的测试代码 

src/main/resources之类的，对应的目录是src/main/resources/archetype-resources/src/main/resources目录，在这里给好你预先包含的配置文件 

filtered参数表示是否对文件启用占位符替换，packaged表示是否将文件放到包路径下。因为package是创建项目的时候必须输入的，就是你的包基础路径。一般是需要将代码放到包路径下的，而资源文件不需要放到包路径下。 

默认情况下，会要求输入groupId，artifactId，version，package，也可以自己额外定义要求输入的参数，都可以在资源文件或者代码中使用${}的占位符方式来引用 

同时对于你写的一些基础的类，一般是可以用package占位符的，到时候创建出来就会替换package 

比如说 

package ${package} 

public class Application { 

} 

（4）部署arthcetype到私服 

mvn clean deploy安装到本地仓库和私服，都可以使用了 

（5）用archetype创建一个工程 

然后就可以用 

mvn archetype:generate -DarchetypeGroupId=com.zhss.archetypes -DarchetypeArtifactId=archetype-oa -DarchetypeVersion=1.0.0 

然后会提示输入各种参数，就会生成一个项目 

3、archetype catalog 

也可以做到不需要输入archetype的坐标就可以使用，就是要将archetype加入一个archetype列表供用户选择，这个archetype列表就是在archetype-catalog.xml文件中。 

maven默认会从几个地方去读取archetype-catalog.xml的内容： 

（1）internal：maven-archetype-plugin内置了几十个archetype

（2）local：从~/.m2/archetype-catalog.xml读取，默认不存在，要自己创建

（3）remote：读取maven中央仓库的archetype-catalog.xml，大概有几百个archetype

（4）file：读取本机任何一个文件

（5）http：读取任何一个网络上的文件 

默认maven会从local+remote去读取archetype列表供选择 

可以用mvn archetype:crawl来自动化扫描本地仓库中的archetype，然后生成一份archetype-catalog.xml，放在~/.m2/目录下，但是一般不用这种方式。 

除非呢你是要做一个开源的archetype，对不对，然后才去关注这个东西，或者是你写了一个archetype，要整合到eclipse里面去，这种都很麻烦，冷门的知识，我都不想讲。 

### 38_最终的历练：手把手带你看懂mybatis源码中的pom.xml

1、针对3年工作经验以内的同学，去任何一个公司，对那个公司的所有mavan相关的东西，一过去，跟同事拷贝一个settings.xml文件，然后下载了项目，看到了pom.xml文件，立马就知道里面所有的配置是干嘛的。不至于说在一个公司干了几年，对那个公司的maven相关的东西，还是模模糊糊，一知半解的 

2、能够如果你自己作为一个架构师，要主R一个项目，那么你对maven这块东西，完全可以搞定，包括怎么配置项目的settings.xml，怎么弄项目的父子工程，版本怎么约束，pom.xml这一套，怎么玩儿，maven的version是怎么递进的 

3、你看任何一个maven管理的开源项目的源码的pom.xml，完全知道里面的内容是怎么回事儿，很清楚，绝对没问题 

4、在我们后面学习的课程中，对任何maven的东西，我都不再讲解了，上手就直接干。而且碰到各种乱七八糟的maven相关的问题，你们自己都可以搞定。 

https://github.com/mybatis/mybatis-3 

<scm>和<issueManagement>相关的东西，是跟一个插件，releases插件，每次要发布一个版本的时候，有一个自动化做各种检查，以及更新版本号的一个插件，跟git仓库结合起来使用的，等到git也讲完了之后，在正式做我们的大电商项目的时候，会给大家演示的 

毕竟git这一块还没讲解，所以过多涉及git的内容，也比较尴尬 

<ciManagement>也是，在大电商讲解的过程中，会跟jenkins持续集成结合起来讲解，在微服务架构那块会有的 

site这个生命周期，我说句实话，可能一些开源项目比较常用，在公司里做项目，很少说基于他的site去生成文档的

### 39_课程总结：一张思维导图让你记住掌握的maven技术体系

我们掌握了什么能力？ 

有哪些遗漏了没有讲解？ 

1、冷僻知识点

2、跟持续集成的整合，后面讲解

3、根git的整合，自动化管理版本，后面讲解

4、生成站点信息，过时了，一般在jenkins上看，后面讲解

5、m2eclipse，功能，建议少用，多用命令行把，bug较多，有奇怪问题

6、手写maven插件，很少用，较为冷僻，建议用到的时候自己去看下demo 

思维导图

授人以鱼不如授人以渔
	百度
		maven报错
		其他任何你想了解而我没有讲解的
	官方文档
		插件细节的使用
	《maven实战》电子版pdf
		优点：大而全
			没涉及到的冷僻知识点
		缺点1：过于学术，不利于学习
		缺点2：没有按企业实战的角度

没涉及到的东西
	maven+git整合的使用
		后面做项目会讲解
	maven+持续集成的使用
		后面做项目会讲解
	maven site
		生成项目文档站点
			API：类、方法、变量
			项目信息：缺陷，测试报告
		适合开源项目
		不打算讲解
		根据兴趣自行查阅资料
	maven插件开发
		讲解过了如何使用搜索和常用插件
		公司有需求
		根据个人需求自行查阅资料
	冷僻知识点
		有些功能一些细节的语法
		很少用的一些东西

为什么需要
	学员基础问题
	效率问题
	依赖和构建的自动化管理

快速入门
	windows安装
	快速体验的hello world
	体系结构
	eclipse+maven
	eclipse上的maven hello world

案例背景
	组织机构模块
	权限管理模块
	流程审批模块
	web模块

依赖管理
	坐标
	深入依赖管理机制
	依赖冲突
	仓库
		仓库架构
		nexus的windows安装
		nexus仓库架构
		在nexus上配置企业级仓库架构
		镜像+repository强制从私服下载依赖
		nexus权限管理
		使用专用账号部署构建包到私服
		nexus的日常管理任务

构建原理
	生命周期+phase+plugin goal
	plugin的配置

工程管理
	依赖->工程化拆分
	聚合->多模块统一构建
	继承->统一约束依赖版本号
	properties->集中约束版本号
	import pom->第三方组件强制约束依赖方版本号

开发流程管理
	单元测试+覆盖率
	本地启动jetty->冒烟测试
	cargo->自动化远程部署
	资源过滤+profile->适配多套环境
	maven&git->版本管理&版本控制
	互联网公司的版本管理

架构师视角
	archetype->公司/项目的统一工程骨架
	开源项目的pom.xml