# 02_git企业级实战

### 01、大白话告诉你什么是版本控制系统以及Git前世今生

Git，版本控制系统，VCS，version control system 

1、什么是版本控制系统？ 

先说说到底什么是版本？？ 

比如说看下面，这个文件，就是HelloWorld.java的第一个版本 

```
public class HelloWorld {
    
    public static void main(String[] args) {
        System.out.println("hello world......");
    }
    
} 
```

修改了一次代码，HelloWorld.java变成了第二个版本 

```
public class HelloWorld {
    
    public static void main(String[] args) {
        System.out.println("hello world......");
    }
    
    public String sayHello(String name) {
        return "hello, " + name;
    }
    
} 
```

过了几天，又修改了一下这个文件的代码，变成了第三个版本 

```
public class HelloWorld {
    
    public static void main(String[] args) {
        System.out.println("hello world......");
    }
    
    public String sayHello(String name) {
        return "hello, " + name;
    }
    
    public String sayHello() {
        return "hello, welcome......";
    }
    
} 
```

抽象一下，什么是版本？一个代码文件，多次修改，每次修改都是这个代码文件的一个版本，每个版本的代码都是不一样的。 

这个版本，就是版本控制系统中，指的那个版本。 

那么版本控制是啥意思？ 

此时HelloWorld.java处于其第三个版本的状态，一共有16行代码。但是此时问题出来了。比如这个HelloWorld.java代码上线运行了一下，后来发现说，不对啊，这个版本的代码好像不是我们所期望的，这个时候如果需要你快速的回到上一个版本的代码，然后重新修改后再次上线。这个时候怎么玩儿？ 

如果你可以通过你的人脑完成几百行，几千行，甚至几万行，几百万行代码，迅速通过你的记忆，还有你手工修改，回退到上一次修改之前的那个代码状态，我可以打赌，你绝对可以上最强大脑。但是问题是，普通的人类，大脑功能没有那么的强大。 

此时，你就需要进行版本控制 

（1）要记录下来一个文件修改过程中的每一个版本 

（2）可以针对每一个版本进行比如说，版本的回退，多个版本的代码的差异的比较。我跟大家举个例子吧，你现在在做一个代码开发，但是发现写完代码之后，坑爹了，就是怎么测试都不通过，都是各种报错。着急了，我现在写的这个代码跟之前提交的代码之间的差异是什么？我到底改了哪些东西。。 

（3）自己做版本控制，行不行？没问题 

（4）有问题，太麻烦了，如果你手头在做一个很大的项目，有几千个，甚至几万个，几十万个代码文件，同时最后又几百万个，几千万个版本，你怎么玩儿？通过手工，根本没法弄 

那么版本控制系统又是啥？ 

有这么一套系统，然后呢，我们是这样，就是说每次修改代码之后，就将这次修改后的这个版本提交到那个版本控制系统中去。然后由这个版本控制系统来给我们管理我们代码中每个代码文件的所有的版本。 

当我们需要进行版本控制的时候，比如回退一个版本，此时直接向那个版本控制系统下达一个命令，说，我现在需要切换回上一个版本的代码，你给我在1s之内完成这个事儿。然后版本控制系统同，就会自动找到上一次提交的版本的代码，然后恢复到你的电脑上来。 

2、版本控制系统的演进历史 

（1）本地版本控制系统 

最早的版本控制系统了，比较知名的是RCS，就是在本地对你的文件的每次修改维护一系列的patch，每个patch就是两个文件版本之间的修改。然后如果你要回到某个历史版本上，RCS会通过path的应用来恢复任何一个时间点的文件。 

这个方式比较Low，常见于上世纪八十年代，就一个程序员写一个系统，编程，只要自己可以管理自己的代码版本即可，不需要跟别人协作。 

但是如果你要跟其他同学去协作共同修改一份代码，来完成某个系统的开发，此时用这种就不可能了。 

（2）集中式版本控制系统 

后来人们遇到的问题就是，不是只有自己需要在本地对文件版本进行控制，尤其是软件开发人员，需要多人写作，对同一份代码进行版本控制。比如每个人都要对代码进行修改，那么每个人都会更新出一个版本的代码。 

这个时候就需要集中式的版本控制系统，就是弄一个服务器，上面放所有代码文件，并且进行版本控制，接着多个开发人员在本地连接服务器，进行代码从服务器检出到本地，代码的修改，以及代码修改的提交到服务器。 

比较知名的有CVS和SVN，很长一段时间内，尤其在上个世纪的90年代到2000年初期，基本就是版本控制的标准。持续到了2005年，2010年之前，还是蛮广泛的。SVN，比如说，大名鼎鼎的百度，当年也是SVN去做代码版本控制。 

但是集中式版本控制系统最大的问题，就在于单点故障，如果服务器故障一段时间，那么那段时间里，各个开发人员几乎啥也干不了了，没法从服务器检出最新代码，没法提交代码。如果更加坑爹的事情发生，就是服务器的磁盘坏了，结果还没有备份，那么完蛋了，所有的代码全部丢失。 

（3）分布式版本控制系统 

这种模式下，每个开发人员自己的本地电脑，都会从服务器检出一份完整的代码包括历史所有的版本到本地。相当于每个开发人员本地都有一份完整的代码版本的副本拷贝。此外，每个开发人员自己本地都有一个完整的版本控制系统。 

如果服务器有任何故障，开发人员在自己本地可以做所有的工作，包括代码版本的切换，代码修改的提交，等等所有的版本控制操作，不会影响自己的工作。如果服务器磁盘坏了，数据丢失，那么可以将某个开发人员本地的版本库拷贝一份到服务器，去恢复数据即可。 

优势是三个 

1）跟集中式版本控制系统不一样的地方，在于说每个研发人员会将服务器上所有的代码和所有的版本都拷贝到自己本地来。但是集中式版本控制系统，就从服务器拉取部分代码和部分版本。 

2）一旦服务器断网，故障了，我们在自己本地可以基于完整的代码和历史版本完成所有的工作；集中式版本控制系统，拉取一些代码，切换一个历史版本，都要联网，基于服务器去搞。联网的话就意味着速度很慢，既有服务器就意味着，服务器故障，就不好搞了 

3）一旦服务器磁盘坏了，代码和历史版本都丢失了，没关系，直接从任何一个研发人员的笔记本电脑上，就可以恢复一份完整的代码和历史版本到服务器上去，可能会丢失一点点最新提交的代码，但是肯定是可以恢复大部分的代码的 

大名鼎鼎的分布式版本控制系统，就是Git 

3、Git前世今生 

linux的创始人，linus，最早就是用很原始的方式在管理linux源码，开源贡献者通过diff工具把修改后的源码发给linus，然后linus手工合并所有代码，自己解决冲突。当时linus始终非常反对使用CVS、SVN等版本控制系统，因为觉得是集中式的，需要联网，很麻烦。 

但是到了2002年，代码过于庞大，在社区的压力之下，linus选择了商业的版本控制系统，BitKeeper，拿到了免费的使用授权。结果不幸的是，2005年的时候，linux社区里一个哥儿们，尝试破解BitKeeper，结果被BitKeeper公司发现了，愤怒收回了对linux社区的免费使用授权。 

接着linus就用2周的时间自己写了一个分布式的版本控制系统，然后linux的几百万行代码就开始使用Git进行版本控制了。一直发展到今天，Git成为了最流行的版本控制系统。直到今天，尤其是互联网公司，基本都是git，还在用svn的话，切换一下。 

Git诞生的前世今生。

### 02、案例背景引入：基于Git实战企业级的OA系统代码管理

学习Git 

我们之前学习maven的时候，已经有一个弄好的OA系统的代码了 

但是当时你可以认为，我们就是没有做任何的代码的版本控制，相当于是直接在代码上进行修改 

如果有多个研发人员的话，那就直接拷贝代码，或者是每个人负责一个工程，不会多人一起去修改一个工程的代码 

OA系统的代码， 去进行企业级的实战，模拟这么一个场景 

我们一开始就是一个很low的小团队，然后没有用任何系统进行代码版本的托管 

接下来，我们就是要随着学习git，然后逐步逐步的使用git来完成整个oa系统的代码托管 

同时完成git企业级环境的这么一个搭建 

最后基于git完成多人协作开发OA系统的整个工作流程的实战和讲解 

git书籍，或者是一些git视频 

比较反感一件事情，为了讲技术而讲技术，这个我觉得是当前知识学习的一个通病，包括写书，视频课程 

就是给你从技术的角度去讲这个东西，先怎么样，一个功能，然后再是第二个功能 

最后可能会给你一个小型的demo案例，只是用到了这个技术的部分功能而已 

有这么一个培训机构，也是出了一个所谓的java架构师的培训系列课程，讲授方式，跟大学里上课一样 

架构师要掌握的技术有几十种 

然后他就给你一个一个技术的讲解，每个技术一个一个功能的讲解 

太枯燥了，讲完以后，这个技术很多东西可能你就忘记了，或者压根儿不知道在项目中是怎么用的 

还是从实战场景出发，去一块儿一块儿的讲解这个技术的所有的功能 

技术的功能，主要是为了解决业务场景的

### 03、在Windows上安装和配置Git

1、建议git命令行

首先是尽量全部在git命令行模式下完成，命令行模式玩儿熟了，才能触及git的本质 

到了后面，当然在企业中开发的时候，那是要用eclipse+git整合，git gui可视化工具，gitlab开源的git服务器去使用 

git命令行是最核心的工具，可以执行git所有的功能，而且可以触及git的本质 

而大多数的git gui工具，只能执行一部分的git命令和功能，而且学会了git命令行之后，对于git gui工具的使用就是轻而易举，因为各种功能背后的命令和原理你都熟悉了 

因此建议学习从git命令行开始，然后实际工作中根据自己的个人喜好选择一种git gui工具使用即可，或者在工作中也可纯碎使用git命令行，感觉很酷 

2、安装git 

直接下载git的windows安装包，一步一步傻瓜式安装即可 

git官网：https://git-scm.com/ 

安装完了之后，就可以得到两个工具，git gui和git bash，git gui就是给你图形化的方式来使用git，但是不推荐现在使用；git bash就是用模拟linux的方式让你在命令行使用git 

打开git-bash，进入一个模拟了linux环境的命令行的界面，然后通过这里就可以去操作git，同时也可以认为git-bash是一个windows上的linux命令行模拟器，因为linux上的大部分命令都可以执行，包括ssh 

git --version，检查一下git的版本号 

3、配置git 

git有一个命令，git config，专门用来对你的git环境进行各种配置。git有三个地方放环境配置： 

之前正好都学习过了maven，在本地安装了maven之后，maven是有自己的一个配置文件，settings.xml，然后每个maven管理的项目中又有一个这个项目的特定的maven配置文件，pom.xml 

git是一样的，全局情况下，也是有git自己的配置文件的 

（1）/etc/gitconfig，对当前机器上所有的用户和git项目都生效，使用git config --system，即可对这个配置文件进行操作，也不是很重要，因为我们一般很少去配置这个文件，因为git的配置一般都是对某个用户和项目的范围去生效的 

（2）~/.gitconfig，当前用户主目录的.gitconfig文件，对当前用户有效，使用git config --global，即可对这个配置文件进行操作，windows的话，是在这里：/c/Documents and Settings/All Users/Application Data/Git 

（3）项目的.git目录下的config文件，仅仅对当前git管理的这个项目有效，直接git config操作的就是这个文件 

因为我们其实一般都不会直接修改git的配置文件来进行配置，一般都是执行git config命令，来修改我们的各个不同的范围的配置，比如全局的配置，或者是项目特殊的配置 

配置用户名和邮箱，安装好git之后第一个事情，就是配置自己的用户名和邮箱，后面每次你提交代码的时候，都会带上你的个人信息 

git config --global user.name "zhss"

git config --global user.email "zhss@163.com" 

git config --global的意思，就是设置当前用户范围内的配置，对机器上的其他用户是无效的

git config --system，就是对当前机器上所有用户都生效

git config，就是对当前所在的git项目本身生效 

接着可以配置默认的文本编辑器，比如输入提交代码时的备注信息时，可能会打开一个文本编辑器让你输入，默认就是用操作系统自带的编辑器。不过这个说实话，一般用操作系统自带的就好了，除非你自己喜欢用别的。 

git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession" 

查看所有的配置项：git config --list

查看某个配置项：git config user.name 

4、帮助文档 

如果对某个git命令不太熟悉，可以用git的帮助文档 

git add --help

### 04、在本地完成Git托管OA系统代码以及本地提交代码

在实战中来学习git如何托管代码，然后如何对代码进行版本控制

OA系统代码  

1、创建一个git版本库    

有两种方式可以创建一个git版本库，将一个版本的项目目录转变为通过git来进行版本控制；也可以从其他地方克隆一个git版本库。 

（1）对一个已有的项目初始化git版本库    

我们一般都是先创建一个项目，这个项目处于一个目录下，此时如果要让git来管理这个项目，就可以在这个项目的根目录下面执行以下命令： 

git init 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0401.gif) 

执行了git init命令之后，意思就是说，要让git托管这个项目的代码，后续这个项目中的所有代码的变动，都可以作为一个版本提交到git中，git会管理这个项目的每个版本的代码，同时后续我们就可以基于git对这个项目进行各种各样的版本控制的功能     

git init命令执行之后，做的第一件事情，此时就会创建一个.git隐藏目录，这里包含了git的所有的文件，就是每个版本的代码都会存储在.git目录中。之前我们不是说，如果要对HelloWorld.java文件进行版本控制的话，实际上是要保留这个文件的多个版本的代码的，如果自己手工做，那么就是保存多个代码文件。 

.git目录中，实际上就是存储了你的每个代码文件的每个版本，每个版本就是一个独立的文件。历史版本都被存储在了.git目录中。 

但是此时git还没有开始对你的项目进行版本控制。如果要对你的项目代码进行版本控制，需要执行以下命令： 

git add --all .

git commit -m 'initial project version' 

把上面两个事情给做了之后，实际上就是代表的是，将项目代码当前的版本，提交到git中去，保存到.git目录中去，开始让git管理当前项目的所有代码文件的后续的版本变更 

有的时候，我们不是对项目里所有的代码或者是文件都要追踪其版本变更的，尤其是对一些自动生成的一些东西，比如说maven生成的target目录下的东西，跟我们的代码是没关系的 

我们是不希望通过git来管理target下面的东西的 

我们可以自己手动创建一个.gitignore文件，在里面每行一个，放入不需要被git托管的目录或者文件 

纠正一下 

我平时工作都是在mac上的 

rm -rf .git

git config --global core.autocrlf false 

git init

git add --all .

git commit -m 'inital commit'

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0402.gif) 

解释一下，这里就显示出来了，将哪些代码文件提交到了git，一共有多少个文件，有多少行，根据.gitignore排除了所有的target目录下面的东西，不要让git去托管target目录里的内容 

到此为止，我们就对这个项目的所有代码文件都实现了版本控制。 

就是我们已经用git托管了oa系统的代码了，oa-parent下所有的代码文件，而且已经提交了第一个版本的代码到git中去了 

再来尝试几次，比如在eclipse里面修改我们的代码，修改好了之后，再次提交到git中去，作为第二个版本，再尝试一个第三个版本 

就是git commit -m后面跟着的一串内容，就是你这次提交的修改后的代码都干了些什么事情，简短一点 

修改oa-web中的代码，加一行日志输出

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0403.gif)

此时oa-parent这套代码，就形成了第二个版本 

再次尝试一下，在oa-auth的server中加入一行日志输出，再次提交

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0404.gif)

发现说，我们明明就修改了一行代码，2个文件改变了，插入了6行，这个我给大家解释一下，有时候可能是eclipse自动生成的一些文件也变化了 

所以我刚才又修改了一下日志打印，然后再次提交

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0405.gif) 

大家会发现说，我们初体验了一把，用git怎么来托管我们的代码，管理代码的多个版本 

（1）git init，初步让git准备脱光我们的代码，创建了一个.git目录，其中是git用来存放版本数据的

（2）git add和git commit两个命令，完成了第一个版本的提交，代码提交到了git中，之后所有这些代码文件的变动，都会由git来管理版本

（3）每次我们修改完一块代码，比如开发了一个小功能，或者是修改了一个小bug，就可以用git add和git commit命令，将修改后的最新版本的代码提交到git中

（4）循环往复，通过每次修改代码+提交修改后的代码到git中，git就完成了对我们的每个代码文件的每个版本的数据的存储，.git目录中

（5）接下来，git才能为我们提供各种版本控制的功能

### 05、图解Git本地仓库结构以及git add和git commit幕后原理

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0501.png)
 git add命令和git commit命令执行的时候，背后的原理是什么？带出来git体系结构 

1、git本地仓库结构 

git项目有3个主要的部分组成：工作区（working directory / working tree），暂存区（staging area），版本库（git directory / repository） 

git directory / repository：git版本库，其实就是git用于存储自己的元数据，以及文档数据库的地方，默认就是在项目的.git隐藏目录中 

working directory / working tree：工作区，保存的是一个项目当前的一个版本对应的所有文件，这些文件是从git版本库中的压缩后的数据库中提取出来，然后放到我们的磁盘上去。 

staging area：暂存区，就是一个文件，包含在git版本库中，主要是保存了下一次要提交到的那些文件信息。在git中，对暂存区有另外一个名称，叫做index，也就是索引。 

上面三个区域的协作关系大致如下： 

（1）首先会在工作区修改某个版本的文件

（2）将某些修改后的文件放入git暂存区中，准备下一次提交到git版本库中去

（3）执行一个提交操作，将暂存区中的文件保作为一个快照保存到git版本库中去 

如果一个文件，已经有一个版本被保存到了版本库，那么就是committed状态；如果这个文件被修改了，同时被加入了暂存区，那么就是staged状态；如果这个文件修改了，还没有加入暂存区，那么就是modified状态。

工作区，working directory 

所谓的工作区，指的就是当前你的git管理的项目，在本地的那个目录，也就是你能直接看到，编辑的那个目录，这就是工作区。 

git add和git commit两个命令幕后的原理 

2、git的重要机制 

Git有很多特有的机制，都是跟普通的版本控制系统不一样的 

（1）快照机制 

普通的版本控制系统，比如说CVS，SVN等，是通过一开始提交一个原始文件，然后后面每次对文件进行修改之后再次提交，都维护这次提交对应的一个差异，通过维护每个版本的差异，就可以通过应用差异，或者回退差异，来前进或者后退文件的版本。 

Git用的不是这种维护每次提交的差异，而是用的快照。每次提交文件，都是保存一份这个文件当前这个状态的一个完整快照，同时对这次提交维护一个指针，指向这个文件快照。 

（2）本地化操作 

大多数的git版本控制操作，只要在本地执行即可，所有的版本文件都在本地，因此操作是非常快速的。相比较于那些依赖网络的集中式版本控制系统来说，他们的大多数操作要依赖网络，速度是很慢的。 

比如说通过git查看提交历史，比较历史文件的差异，都可以在本地完成，不需要通过服务器做任何事情。 

如果我们在飞机或者或者上，没有网；或者在家里，没有vpn。都没有问题，随便做开发、写代码，提交代码，在本地就可以了，等有网络的时候，再把提交的版本推送到远程服务器上去。但是SVN之类的，就不可以提交了，因为没有网，没法连接到服务器。 

（3）完整性保证 

git在存储任何文件之前，都会对其执行一个校验和，然后用校验和指向那个文件。这是git内核保证的，这样我们是不可以手工修改git版本库中的任何文件的，因为修改了文件之后，会导致计算出来的校验和与之前保存的校验和不匹配，文件会破损。 

git用的是SHA-1 hash算法来计算校验和，这是一个40位的字符串，基于文件的内容计算出来的，看起来大概是这样的： 

24b9da6552252987aa493b52f8696cd6d3b00373 

如果手动破坏.git中存储的文件的内容，git会不承认，因为内容变化之后，会导致内容计算出来的SHA-1 40位的hash值变化，跟之前存储的hash值不同，就认为文件破损 

（4）仅仅添加数据 

git通常来说，仅仅会在自己的数据库中添加数据，因此提交文件到git之后，很少会丢失，而且如果我们定期提交文件到远程服务器，就更少丢失。

### 06、结合Git仓库结构用git status查看OA系统的代码状态

上一讲已经给大家剖析清楚了git仓库结构：工作区，暂存区，仓库

git add和git commit在干嘛 

但是结合git中不同的区域，你会发现每一个代码的版本都可能停留在不同的区域中，这就可能处于不同的一个状态下 

所以说，这一讲，我们结合上一讲的内容，来学习一下用git status，来查看当前git项目的各种状态 

1、动手实验git status查看代码文件的状态（结合git仓库结构） 

代码文件分成两种，一种是tracked，一种是untracked。tracked文件就是已经提交到git版本库中的文件，后面可以处于modified或者staged状态；untracked文件，就是从来没有提交到git版本库的代码文件（也从来没有放入暂存区）。 

在oa-auth的server包下，可以新建一个TestServiceImpl.java，然后执行git status命令，看一下当前的状态

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0601.gif)

此时git status会告诉你，有文件没有被提交到git仓库中或者是暂存区中过，就是untracked，就是从来没有被git追踪过它的版本，甚至第一个版本都没有被git追踪。 

接下来执行命令： 

git add oa-auth/src/main/java/com/zhss/oa/auth/service/impl/TestServiceImpl.java  

git add --all .，也可以给大家开始来解释一下这些命令的含义，git add .，就是将当前新增或者是修改过的文件，加入暂存区，但是加了--all之后，如果有文件被删除，也会将文件被删除的状态加入暂存区中 

一般工作中，都是用git add --all . 

但是也可以直接git add 某个文件，把指定的文件加入到暂存区中

 ![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0602.gif)

TestServiceImpl.java这个文件，从untracked状态，变成了new状态，就是一个第一次进入git托管的一个新文件，看到的第一个版本 

git commit -m 'add TestServiceImpl class'

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0603.gif)

此时，这个文件就进入了一个tracked状态，已经被git开始追踪了 

然后对TestServiceImpl.java进行修改，加入一个方法，接着看一下状态

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0604.gif)

此时变成了modified状态，就是已经被追踪了，但是后来又被修改了，但是是changes not staged for commit，就是被修改了，但是没有被staged，staged就是还没有被加入暂存区 

再次执行git add --all .命令

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0605.gif) 

此时的状态是，changed to be committed，modified，此时文件被放入了暂存区，modified状态，同时等待被commit提交到git仓库中去

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0606.gif)

此时文件变成了committed，已经提交了 

总结一下

（1）新文件刚创建：untracked，此时仅仅停留在工作区中

（2）git add 新文件：new file，此时已经被追踪了，放入了暂存区中

（3）git commit 新文件：committed，已经被追踪了，放入了git仓库中

（4）修改那个文件：modified，changes not staged to be committed，没有加入暂存区，被修改的内容仅仅停留在工作区中

（5）git add 修改文件：modified，changes to be committed，修改的文件版本被已经加入暂存区

（6）git commit 修改文件：committed，修改后的新版本提交到了git仓库中 

一般自己创建的版本库，刚开始文件都是untracked，然后git add和git commit命令执行之后，就被提交了第一个版本到git版本库，此时就全部都是tracked了。如果是从远程版本库克隆下来的，那么刚开始就是tracked。 

接着对tracked的文件修改之后，就是modified；然后对modified文件再执行git add命令之后，就是staged，进入了暂存区；接着执行git commit命令之后，就将暂存区中的文件都提交到了版本库中，此时就是unmodified，and tracked。 

2、6种状态映射到3种状态的补充说明 

git管理的文件有三种状态：提交状态（committed），修改状态（modified），暂存状态（staged）。 

提交状态：我们的文件已经安全的保存在git的本地数据库中了。

修改状态：我们修改了文件，但是还没有提交到git的数据库中去。

暂存状态：将修改后的文件标记为即将通过下一次提交，保存到git数据库中去。 

（1）新文件刚创建：untracked，此时仅仅停留在工作区中

（2）git add 新文件：new file，此时已经被追踪了，放入了暂存区中 => staged

（3）git commit 新文件：committed，已经被追踪了，放入了git仓库中 => committed

（4）修改那个文件：modified，changes not staged to be committed，没有加入暂存区，被修改的内容仅仅停留在工作区中 => modified

（5）git add 修改文件：modified，changes to be committed，修改的文件版本被已经加入暂存区 => staged

（6）git commit 修改文件：committed，修改后的新版本提交到了git仓库中 => committed 

3、补充实验，彻底了解git add和git commit的关系，包括3个区域的关系 

（1）首先我们先修改一下TestServiceImpl这个类，加入一行代码 

（2）执行一下git add命令 

（3）再次修改一下TestServiceImpl这个类，再加入一行代码 

（4）直接执行git commit命令

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0607.gif) 

大家必须建立起来一个版本的概念 

（5）解释说明 

第一次加入一行代码：System.out.println("invoke test method......");  

这是一个版本，v3，然后git add之后不，就是带着这一行的v3版本放入了暂存区 

接着你又添加了一行代码，System.out.println("prepare to return value......"); 

这又是一个版本，v4，但是v4，带着System.out.println("prepare to return value......");行代码的版本，没有被放入暂存区 

此时直接执行git commit操作，是将暂存区里的v3版本的代码，就只有一行System.out.println("invoke test method......");代码，提交到了git仓库，此时就是认为，就插入了一行而已 

git commit操作，只会将已经放入了暂存区的代码提交到仓库 

你带着System.out.println("prepare to return value......");行代码的v4版本，还停留在工作区呢，changes not staged to be committed。 

你需要再次git add + git commit，才能将v4版本提交到git仓库 

总结一下，对于git来说，什么是一个版本？ 

一次git add算做一个版本，每次执行一次git add操作，都是将工作区中所有修改的文件，都作为一个新的版本，放入暂存区，这个版本等待被提交 

为了让大家不要迷糊 

最后说明一下，就是实际工作中，一般很少说多次git add，过了很久，搞一次git commit 

说真的，一般其实每次git add和git commit都是一起执行的 

就是你修改了多少代码，然后你希望作为一个新的版本，去提交，那就一次性git add，然后git commit就可以了，一次git add + git commit，就出来了一个新的版本 

必须git add到暂存区里的修改，才会被git commit时提交到仓库里，否则就是停留在工作区中 

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0608.png)

###  07、基于git log查看以及深入图解Git提交历史

1、基于git log查看提交历史 

在我们提交了很多次之后，可以查看提交历史，使用git log命令即可，可以看到每次commit的信息，包括了commit的SHA-1、作者、日期、提交说明。  

包含了一个commit的SHA-1 hash值，40位的字符；作者，从之前安装好git之后做的设置来的；date；提交备注，inital commit，git commit -m ''

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0701.gif)

通过git log命令，我们可以看到到目前为止，一共是10次提交，尤其注意看一下最近一次提交

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0702.gif)

（HEAD -> master）是什么意思？      

版本控制系统中，都有一个概念，叫做分支，就是同样的一份儿代码，可以拉取多个分支，每个分支的代码刚开始都是一样的，然后不同的人可以基于不同的分支去开发，然后开发完之后，再将多个分支的代码合并在一起 

分支，就是一份代码拷贝，基于这份独立的代码拷贝，去写自己的代码 

最后将多个分支合并在一起，就是将不同的人写的代码合并到一起 

git里面，每次git init之后，默认初始就创建一个分支，叫做master分支 

我们每次写代码，然后git commit提交，都会形成一次新的commit，然后默认就是在master分支对应的代码拷贝上，进行代码修改和提交的 

每次提交之后，master分支就会对应着最新的一次提交 

HEAD，指针，指向了我们当前所处的分支，因为当前我们默认就处于master分支上，所以HEAD指针就是指向master的 

git log --patch -2，--patch可以显示每次提交之间的diff，同时-n可以指定显示最近几个commit。这个是很有用的，可以看最近两次commit之间的代码差异，进行code review是比较方便的。 

用git log --stat，可以显示每次commit的统计信息，包括修改了几个文件，有多少行插入，多少行删除。 

用git log --pretty=oneline，可以每个commit显示一行，就是一个commit SHA-1和一个提交说明。用git log --pretty=format:"%h - %an, %ar : %s"，可以显示短hash、作者、多长时间以前、提交说明。 

用git log --oneline --abbrev-commit --graph，这是最有用的，可以看到整个commit树结构，包括如何合并的，就显示每个commit的SHA-1和提交说明，同时SHA-1显示短值。 

--oneline：显示一行，不要显示多行那么多东西，一行里，就显示commit的标识符，SHA-1 hash值，40位的；提交备注；显示分支和HEAD指向哪个commit 

--abbrev-commit：commit的标识符，每一次commit，都有一个唯一的标识符，就是一个SHA-1 hash值，40位，显示一个短值，默认显示前7位，就是说前7位就可以唯一定位这个commit了，不需要完整的40位 

--graph：显示图形化的commit历史，这个大家后面学习到分支那里就知道了，如果有分支的话，commit历史会形成一棵树的形状，这个时候用--graph可以看清楚这颗commit树长什么样子，很有的 

告诉大家，通过git log命令，就可以看到你多次往git仓库中提交不同代码版本的整个历史 

2、深入图解git提交历史 

就是你每次git add之后，git commit之后，会在git仓库里存储什么东西，存储的那些东西跟分支还有HEAD之间的关系是什么？？？ 

用一张图画清楚，工作区、版本库、暂存区、master分支以及HEAD指针的关系 

工作区和版本库是两个分离的区域 

在工作区修改代码之后，执行git add命令，会将代码放入版本库中的暂存区；接着执行git commit命令之后，会将暂存区中的代码提交到版本库中的master分支上，而HEAD指针就指向master分支的最新一次提交。 

所以现在我们就很清楚了，git add，其实就是可以多次修改代码，多次git add，然后将每次修改的代码，都放入暂存区中；而git commit，就是一次性将暂存区中的代码，全部提交到master分支上，master分支会出现一个最新的commit，也就是一个最新的代码版本；而HEAD作为一个指针，永远指向master分支的最新一次commit的代码版本。

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0703.png)

### 08、结合Git提交历史用git reset体验多版本代码切换

 上一讲，我觉得就已经比较透彻的讲清楚，版本是怎么回事了 

所以你可以理解为，一次commit操作就是一个版本，因为这个commit对应了本次最新的所有的修改 

之前所有的commit做出的修改，在这次commit并没有变化，所以说呢，本次commit包含的最新的所有的修改，加上之前的commimt做出的但是本次没变化的修改，结合起来，不就是最新的一次版本吗？ 

以此类推呢？ 

上一个commit对应的变化，加上它之前的所有commit做出的变化，结合起来，就是相当于是上一个commit对应的整个项目完整的一个版本 

来做一个实验，版本控制 

第一讲，首先git第一个功能，就是通过提交这个事情，保存下来每个文件完整的历史版本 

然后呢，我们如果要做版本控制，比如说，发现这次这个版本不ok啊，希望回退到上一个版本，不要这次最新的这个版本的代码了，打算重新写 

这个时候怎么办？ 

就涉及到了版本的控制 

git reset --hard HEAD^，就可以回退到上一个版本 

HEAD^，代表了什么？ 

HEAD -> master -> commit（add methods for classes） 

HEAD^，代表的是commit（add methods for classes）的上一个commit（add two files） 

git reset --hard HEAD^，就是将仓库、暂存区、工作区，全部恢复到上一个commit（add two files）对应的状态 

git reset --hard HEAD~5，退回到HEAD之前的倒数第5个commit的状态 

git reset --hard d324644，指定一个commit的hash值，回退到很老的版本 

git reset这个命令，可以任意穿梭到历史的任何一个版本上去 

如果我要回来呢？重新回到之前add methods for classes那个commit对应的版本 

git reflog 

有一个时光机，可以任意穿梭，回到过去，回到未来 

实现了我们第一讲所说的，版本管理的困难，被git轻易的解决了 

（1）记录下来了每一个版本

（2）可以让我们通过简单的命令，在任何一个版本中任意穿梭，实现了版本控制功能 

git最核心的原理 

（1）三个区域：工作区、暂存区、仓库

（2）提交历史：在仓库中，blob->tree->commit->master-HEAD，这套东西形成了git的数据结构

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0801.png) 

###  09、基于远程Git仓库的多人协作开发流程


 大家可以想一下，我们到现在为止已经可以搞定哪些事情了 

我们在本地已经用git托管OA系统项目的代码了，git可以追踪oa项目中每个文件的版本的变更，而且我们还可以进行某些版本控制功能，比如版本回退，等等 

但是这里有一个问题，就是我们在企业中，基于类似git这种分布式版本控制系统，是要进行多人协作开发的，不是说回到上个世纪80年代，然后就一个人，在自己本地开发一个软件，那么其实只要在自己本地进行版本控制就可以了 

我们现在是一个团队，假设是两个人，比如我是张三，还有一个哥儿们是李四，也要一起开发的 

所以基于目前的这个现状，是不ok的 

比较合理的是，就是需要搭建一台专用的git服务器，然后呢在git服务器上面建立一个远程的git仓库，然后我们从本地将自己的项目代码先推送到远程git服务器上的仓库中去

然后其他开发人员，就可以从git服务器上克隆一份代码的副本到自己本地，然后两个人就可以直接在自己本地的git环境下进行开发和提交，等版本控制 

然后在每个人开发好一块功能之后，就可以将自己的代码推送到git服务器上去，其他研发人员可以从git服务器上拉取别人推送上去的代码 

具体的多人协作开发，有很多种模式，基于分支

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\0901.png) 

### 10、基于centos搭建Git服务器以及OA系统代码的上传和下载

centos

第一步，在centos上安装git 

git --version

yum install -y git 

1.7版本，还好 

第二步，在centos上创建一个git用户来运行git服务： 

```
groupadd git
adduser git -g git 
```

第三步，每个工程师生成自己的ssh key公钥并放入git服务器 

每个工程师都需要通过ssh-keygen -t rsa，然后在~/.ssh目录下可以找到id_rsa.pub文件，就是公钥，把公钥导入到git服务器上的/home/git/.ssh/authorized_keys文件里，一行一个。 

第四步，初始化Git仓库： 

使用/srv/oa-parent.git目录作为远程仓库，在/srv目录下输入命令： 

git init --bare oa-parent.git 

Git会创建一个裸仓库，裸仓库没有工作区。因为服务器上的Git仓库就是为了共享，不会让用户直接登录到服务器上去修改工作区的，而且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为git： 

chown -R git:git oa-parent.git 

第五步，禁用shell登录： 

一般出于安全考虑，第二步创建的git用户是不允许使用shell的，我们可以编辑`/etc/passwd`文件。 

找到类似下面的一行：

git:x:1001:1001:`,,,`:/home/git:/bin/bash 

改为：

git:x:1001:1001:`,,,`:/home/git:/usr/bin/git-shell 

这样，`git`用户可以正常通过ssh使用git推送和下载代码，但是无法通过shell登录的，因为我们为`git`用户指定的`git-shell``是`每次一登录就自动退出。 

第六步，基于git服务器和远程仓库上传和下载代码： 

git remote add origin ssh://git@192.168.31.244:/srv/oa-parent.git 

将本地仓库和git服务器上的远程仓库关联起来 

git push -u origin master

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1001.gif)

-u，将本地仓库的master分支和远程仓库的master分支关联起来 

git clone ssh://git@192.168.31.244:/srv/oa-parent.git 

（1）搭建起来了一个git服务器

（2）在git服务器上创建了一个远程仓库，bare裸仓库

（3）将本地的oa项目代码推送到了远程仓库

（4）在本地另外一个目录，模拟成另外一个研发人员，在自己本地将远程仓库中的oa项目代码克隆到了本地

### 11、功能分支工作流实战以及深度图解分支内幕原理（一）

本地分支操作内幕原理：

 ![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1104.png)

 功能分支流：

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1105.png)

### 12、功能分支工作流实战以及深度图解分支内幕原理（二）

这一讲，非常非常非常的重要 

因为在git学习中，我觉得最重要的是两块： 

第一块，就是前面的10讲，里面讲解了3个区域，6种状态，提交历史，等等； 

第二块，分支（背后的原理，合并的原理，解决冲突的原理，远程分支的原理） 

全部放在这一讲，用一个比较经典的多人协作开发模型，就是功能分支工作流，实战多人去开发OA系统的代码，然后演示整个分支是怎么玩儿的，每做一步分支的操作，幕后的原理图，对应着画出来 

1、分支的基础知识 

（1）分支的介绍 

所谓的分支功能，就是可以同时拉出来多个代码副本，然后在不同的代码副本上，可以进行对应功能的开发。完成开发之后，可以将多个分支合并在一起，形成最终的代码。 

分支，其实就是指针 

在git中，每一个项目，不管你有多少个分支，不管你在哪个分支上开发 

最终都会形成一个完整的提交历史，树形结构 

每个分支，其实就只是一个指针而已，分支就指向了提交历史中的某个commit object 

每个commit object就代表了这个项目的所有代码在那次提交的时候一个完整的快照版本，包含了之前没有变更的代码文件，也包括了这次提交的最新的修改/新增/删除的代码文件 

每个commit object就代表了项目的一个版本 

所以你的分支实际上就是指向了历史上某个时刻的一个版本的代码，就是一个commit object 

Git的分支功能是一个很大的特色，非常的轻量级，分支的来回切换速度是很快的，在实际的开发过程中，分支绝对是最重要的功能。 

（2）commit object与分支的原理 

Git并不是存储一系列的文件差异，而是存储一系列的文件快照的。实际上每次我们执行一次commit，git都会存储一个commit object，这个commit object中会包含一个指针，指向这次提交文件的快照。这个commit object同时也包含作者的姓名和邮箱，提交说明，以及对上一次commit object的指针。 

将一个文件版本放入暂存区的时候，就会计算一个校验和，然后提交的时候会将文件内容以blob的方式放入版本库中，同时在暂存区放入这个文件版本的校验和。接着git会创建一个commit object，其中会包含元数据，以及一个指针指向版本库中的文件快照。 

也就是说，每次执行一次提交，都会在版本库中包含这么几个东西：一个blob，每个文件都会有一个blob来存储这个文件的本次提交的快照；一个tree，这个tree会包含对本次提交的所有文件的blob的指针；一个commt object，指向了tree的指针，作者，等信息。 

接着如果再次执行一个提交，那么下一个提交同样会包含那些东西：每个文件一个blob，一个tree指向所有blob，一个commit object指向那个tree，同时这个commit object会有一个指针，指向上一个commit object。 

最后多次提交，就会得到一颗完整的commit树。 

分支是啥？分支就是一个轻量级的指针，默认的分支是master，那么每次提交，master分支的指针默认就是指向最新的那个commit object的。每次提交一次，master指针就会挪动，继续指向最新的commit object。 

同时如果你当前工作在某个分支上，比如工作在master分支上，那么git还维护了一个特殊的指针，HEAD，这个指针指向master分支指针。如果你创建了其他的分支，那么其他的分支也会指向某个commit object，而且此时如果工作在那个分支上，那么HEAD指针会指向那个分支。 

（3）创建一个分支 

git branch testing，可以创建一个新的分支，此时这个分支的指针会指向当前你所在的分支所指向的commit object上。 

如何查看各个分支指向哪个commit object呢？使用git log --oneline --decorate命令即可，会给你显示出来。 

（4）切换分支 

git checkout testing，此时就可以切换到testing分支，此时HEAD指针会指向testing分支指针。 

此时如果在testing分支上提交代码，那么会commit树会长出来一个新的commit object，而testing分支指正会指向最新的commit object，HEAD继续指向testing分支指针，而master指针还是指向之前的那个commit object。 

git checkout master，会切换回master分支，此时HEAD指针会指向master指针，同时将master指针指向的那个commit object，对应的tree和其中的blob，也就是对应的文件快照恢复到工作区中来。 

再次在master分支上提交一个修改，此时从这个commit object会再长出来一个新的commit object，看起来就是跟testing分支当前指向的commit object形成了两个分叉。 

此时如果要查看commit树，可以用命令：git log --oneline --decorate --graph --all，这个命令很有用，可以打印出整颗commit树，同时告诉你各个分支当前指向哪个commit object。 

在Git中，分支升级上就是一个很简单的文件，其中包含了一个40位的SHA-1校验和，就是分支指向的那个commit object的SHA-1。所以创建分支的代价是很低的，不过就是创建这么一个文件罢了。而一些集中式版本控制系统，对于分支可能需要拷贝一个完整的文件副本，导致速度很慢，磁盘空间占用很大。 

（5）远程分支 

在本地你可以创建一个分支，然后开发代码，但是后续的话，你肯定是要将本地分支推送到远程仓库里面去的 

git push -u origin 分支名称 

这个的意思，就是将本地分支推送到远程仓库里，形成一个同名的远程分支，同时-u这个选项就是将本地分支和远程分支关联起来 

下一次如果修改了这个分支的代码，直接执行git push origin 分支名称，就直接可以将本地分支的代码推送到远程分支 

如果其他人要将某个远程仓库的分支拉取下来，应该执行一个命令，叫做git fetch origin，就会抓取下来远程仓库新增了哪些分支 

接下来可以执行一个命令，git checkout -b 本地分支 origin/远程分支，用这个命令，origin/远程分支，就代表了远程仓库里的那个分支，然后这个命令一执行，就是在本地创建一个远程分支对应的本地分支，互相关联起来 

以后呢，每次如果别人更新了那个分支的代码，push到了远程仓库，你可以执行git pull命令，将这个分支在远程仓库的代码拉取下来，跟本地分支的代码进行合并 

远程版本库的分支，在本地都有追踪分支，remote-tracking 分支。 

比如说本地的master分支，对应的远程分支就是origin/master。比如本地的feature/iss53分支，对应的远程分支就是origin/feature/iss53。 

假设我们公司的git服务器地址是git.zhss.com，然后如果我们用git clone命令从这个git服务器克隆了一个版本库下来，git默认会将远程版本库命名为origin，同时在本地创建一个指向远程版本库的master分支的本地分支，叫做origin/master。此外，git也会给你在本地创建一个master分支，内容就是跟origin/master分支一样的。 

从git服务器克隆版本库下来的时候，远程版本库的commit树会一同拷贝下来，然后origin/master指向的commit，就是远程版本库的master指向的commit，同时给本地创建的commit也是指向这个commit。 

此时，如果在本地你做了不少开发，然后本地master移动了好几个commit，同时origin/master还是指向最开始的那个commit；而同时，远程版本库上，其他同事也提交了几次代码，因此远程版本库上的commit也移动了几个commit。 

此时如果要让本地和远程保持同步，需要使用git fetch origin命令，该命令会将远程版本库的commit树和所有的分支都拉取下来，跟本地的commit树进行合并，此时可能就会在本地形成一棵有两个分叉的commit树。 

本地的origin/master会指向远程版本库的master指向的那个commit，本地的master继续指向之前本地最新的那个commit。 

使用git push origin serverfix，可以将你在本地的分支推送到远程版本库上去，此时你本地的commit树也会推送到远程版本库，跟远程版本库的commit树进行合并。 

下一次别人执行git fetch origin的时候，就会获取到一个remote -tracking分支，origin/serverfix分支，这个远程分支是只读的。接着你使用git checkout -b serverfix origin/serverfix命令，就可以在本地获取到一个分支跟origin/serverfix关联起来，然后就可以跟你一起对一个分支进行修改了。 

使用git checkout -b serverfix origin/serverfix命令，创建出来的本地分支叫做tracking brach，而这个本地分支track的远程分支叫做upstream branch。此时本地分支就会track那个远程分支，跟远程分支之间会建立关联关系。 

如果我们在tracking分支上，执行git pull命令，git就会自动将对应的远程分支在远程版本库上的代码拉取下来跟本地的tracking分支做合并，如果有冲突的话，还需要解决冲突。 

如果我们是使用clone命令克隆的远程版本库，那么默认就会将本地的master分支创建为追踪origin/master远程分支的。 

使用git branch -u origin/serverfix，可以让当前本地分支track某个指定的远程分支。 

使用git branch -vv，可以查看每个本地分支track的远程分支。 

git fetch origin命令，仅仅就是将远程版本库的commit树拉取下来，同时拉取下来所有最新的远程分支，给我们使用。 

如果我们已经建立起来了本地分支和远程分支的track关系，那么就可以直接使用git pull命令，将远程分支的代码拉取下来合并到本地分支。但是通常来说，我们也可以先git fetch origin，然后git merge 远程分支，一样可以将远程分支代码合并到本地分支上去。 

删除远程分支，比如一个远程分支，已经结束了工作了，可以删除一个远程分支，使用git push origin --delete serverfix即可。同时可以使用git branch -d serverfix删除本地分支。 

2、功能分支工作流介绍 

工作流，git工作流，简单来说，就是多个人如何同时基于git远程仓库，加上分支，去非常良好的协作开发，包括去执行各种集成测试，QA测试，预发布测试，生产环境上线 

第一个工作流，功能分支工作流，比较适合用于5人左右或者以内的小团队 

集中式工作流、功能分支工作流、GitFlow工作流、互联网公司一种GitFlow工作流的变种 

每种工作流都没有绝对的好坏，主要是适用于某个场景下，某种类型的项目 

一般来说，都会准备一个master分支，作为你的稳定分支，这里的代码是随时可以上线的；有多个feature分支，每个feature分支用来开发一个功能。 

如果线上有bug，一般分为两种，一种是影响正常用户使用流程的紧急bug，就是hotfix，比如说电商网站无法下单了，门户网站无法查看新闻了；一种是不影响正常用户使用流程的非紧急bug，比如说有某个广告显示的位置出现了偏差，就是bugfix。 

一般线上发现了bug，就了一个hotfix或者bugfix分支，然后在分支上复现bug，修复bug，然后在测试环境进行验证以及回归测试，最后将分支合并到master去上线，修复线上问题。 

这边，我根据过往的经验，给大家说一下，我觉得这个工作流比较适合什么样的项目？ 

非常适合对公司内部的系统、平台或者工具，5人以内小团队，3人左右，去开发一个比如公司内使用工具，报表平台 

报表平台，不是什么核心的系统，可能就是投入两三个人，就维护一个系统，各种在里面加入功能，满足大家看报表的需求，平时有bug就修复一下。 

新的需求频率也不是很高，可能就是比较稳定的新需求一步一步提出来，大家就比较稳的在哪儿维护这个报表平台就可以了 

分布式爬虫系统，3个人在搞，也是这样子，就是3个人开发和维护一个小爬虫的系统，然后有需要就在里面加入功能，有bug就修复 

没有什么版本的概念，对公司内部，1.0，1.1.2，1.5.3，不需要这么复杂的版本 

就是就很简单，就是一个系统，比如说，你的需求方，某个看数据的运营同学，或者是产品经理同学说，好，现在你的报表系统能不能给我显示一个折线图，因为之前只能是柱状图 

然后你说，好，没问题，拉一个feature分支下来，feature/line_graph，开发开发，测试，最后feature/line_graph合并到master分支，上线。。。 

然后平时有人突然说，饼状图显示不出来了，有bug，bugfix/bar_graph_cannot_show，复现，修复，测试，最后bugfix/bar_graph_cannot_show合并到master分支，上线

3、功能分支工作流实战 

我们来按照真实的网站开发过程体验一下分支的使用和合并过程： 

（1）开发一个OA系统，俩人，张三和李四 

张三：/f/development/eclipse/eclipse-workspace/oa-parent

李四：/f/development/workspace/oa-parent 

（2）张三从master分支上，拉一个新的feature分支，叫做feature/001，然后在分支上做一些开发，开发新功能；李四从master分支上，拉一个新的feature分支，叫做feature/002，然后在分支上做一些开发 

拉一个分支下来：git checkout -b feature/001，就会从当前分支拉一个分支出来

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1201.gif) 

比如你现在在master分支上，然后你执行了git checkout -b feature/001这个命令，此时会创建一个新的分支对应的指针，feature/001，指向了master当前指向的那个commit object，同时如果切换到了feature/001分支，HEAD会指向feature/001 

git为什么要这么玩儿？ 

git不会说，每次创建一个分支，就直接拷贝一份代码，效率太低了 

在git中，整个所有的版本和文件只存储一次，只存储一份，提交历史，commit树，只有一份 

每个commit object就代表了这个项目在当前时刻一个完整的快照版本，代表了一份完整的代码了 

所以创建分支，就是创建一个轻量级的指针而已，指向了某个commit object，你的当前所处的分支指向哪个commit object，就是指向了哪个版本的代码，你的工作区中的代码就是那个版本的代码 

李四：git checkout -b feature/002 

张三进行开发提交了两次，oa-auth里面加入新功能

李四进行开发提交了一次，oa-doc里面加入新功能 

（3）这个时候张三接到一个通知，有一个bug，需要进行hotfix。这个时候我们需要切换到master分支上，拉一个hotfix分支，拉bugfix/001。在hotfix分支上修复bug，然后测试和验证一下，合并到master分支，用master分支代码部署，解决线上bug。删除bugfix/001分支 

张三：git checkout master，git checkout -b bugfix/001，修改bug，提交，再将bugfix/001分支合并到master分支 

如果你要git checkout master，首先需要将当前分支上修改的代码全部git add然后git commit，才能切换分支 

合并分支：切换到master分支，然后执行git merge bugfix/001

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1202.gif) 

这里执行的是一种merge，叫做fast-forward，快进式merge 

我们此时就不需要bugfix/001分支了？为啥，因为master分支就指向了那个commit了，要bugfix/001分支还有何用？ 

给删除掉bugfix/001分支：git branch -d bugfix/001 

（4）张三继续切换回feature/001分支，继续开发工作，开发完之后合并到master，删除feature/001分支

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1203.gif) 

3-way merge，三路合并 

还没有完，张三还需要将feature/001分支删除 

（5）李四同时抑制在开发feature/002分支，完成之后合并到master，删除feature/002分支 

（6）张三要将自己本地的master代码推送到远程去 

git push origin master 

因为本地的master跟远程分支origin/master是关联起来的，origin/master就对应着远程仓库的master分支 

所以执行上述命令之后，实际上会做两件事情，第一是修改本地的origin/master分支，第二是将本地的commit提交历史推送到远程仓库，修改远程仓库的master指针

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1204.gif) 

origin/master和master是关联起来的，只要你一执行git push origin master命令，首先，在本地，就会将origin/master的指针进行移动，移动到和本地的master分支一样 

同时的话呢，会将本地的commit提交历史推送到远程仓库里面去，然后远程仓库的master也是指向了最新的那个commit 

（7）李四也要提交代码，推送代码到远程仓库里去 

git push origin master

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1205.gif)

解释一下上面这段话，意思就是说，远程的仓库里的master分支的代码，已经有人提交过了，然后此时你是不能直接推送新的代码到master分支的 

此时要求你是执行git pull命令： 

第一件事情，将远程仓库的commit提交历史拉取下来跟自己本地的提交历史进行合并 

第二件事情，将本地的master分支对应的commit跟远程仓库的master分支对应的commit进行合并

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1206.gif)

git push origin master 

（8）张三做最后一个操作，就是执行一次git pull，将远程仓库的提交历史拉取到本地进行合并，同时将远程仓库的master跟本地的master进行合并，让本地仓库跟远程仓库保持一致 

4、分支管理命令 

git branch命令，直接执行，就会显示出当前所有分支列表，以及你在哪个分支上工作 

git branch -v命令，可以显示出每个分支当前指向的commit object 

git branch --merged，可以看到哪些分支被merge进了当前分支；git branch --no-merged，可以看到哪些分支还没有被merge进当前分支 

如果git branch -d命令删除一个分支，可能会提示你那个分支还没merge到当前分支来，不让你删除该分支，此时可以使用git branch -D命令，强制删除一个分支。 

如果你有一个分支，还没有合并到master分支去，此时git不让你删除的 

git branch -D，强制删除一个没有合并到别的分支的分支 

梳理 

（1）git分支的一些基础知识：commit提交历史，分支就是指针，HEAD指向当前分支

（2）功能分支工作流是什么东西，怎么玩儿的

（3）功能分支工作流的实战，里面包含了所有常用的分支的命令

（4）接着功能分支工作流的实战，整个git分支幕后的动态原理深度图解剖析，彻底100%讲清楚了，知道，你做的每一步分支操作，对应的git log显示的历史，应该你的脑海中甚至都可以画出一幅图来 

拉不同的分支，就是不同的指针，基于指针指向的那个版本的代码，你在分支上做开发提交，其实就是在commit提交树上长出来不同的枝丫 

切换分支，就是切换HEAD指向的分支指针 

分支合并，其实就是将两个分支当前指向的两个commit的代码，合并到一起，形成额一个新的commit 

origin/master对应的就是远程仓库的master 

本地仓库和远程仓库之间推送和拉取代码，实际上就是在同步整套commit提交历史，包括进行一些分支指针的合并 

5、分支合并冲突 

（1）分支原理再次回顾 

分支管理，是非常重要的 

因为一般来说，都是多个人在协作开发，每个人可能做一个功能，这个时候每人都会并行开发一块代码。那么就需要为每个功能创建一个代码分支，那个分支有完整而且独立的代码，每个人在自己的分支代码上开发，不影响任何其他人。最后都开发完了，再将几个分支的代码合并在一起。 

每个代码分支，都会有多次提交，然后每个代码分支都会处于最新一次提交的代码，这个分支的多次commit，就会形成一条时间轴一样的东西。 

默认情况下，git给我们创建一个master分支，每次提交都会提交到master分支。 

而master分支是指向最新的一次commit的，然后HEAD指针是指向master分支的，是这么一个关系。每次提交到master，master分支就会新增一个commit，然后master是指向最新一次commit的，同时HEAD又始终指向master。画图说明。 

此时如果我们从master分支创建一个其他的分支出来，那么此时git会创建一个指针，叫做dev，这个dev是指向master分支最新一次commit的，然后HEAD指针会指向dev指针。此时如果我们再提交代码，就是提交到dev分支了，此时commit时间轴会延长一个节点，同时dev指针指向最新的commit，HEAD依然指向dev。画图说明。 

git checkout -b dev，这个命令就是创建dev分支，dev指针指向最新一个commit，同时HEAD指针指向dev指针 

git checkout -b，相当于两个命令，git branch dev，git checkout dev，先创建分支，再切换到分支 

接着用git branch命令，可以查看当前的所有分支以及我们目前所处的分支 

接着我们可以在dev分支上修改代码，在HelloWorld.java中增加一行：System.out.println(“I like maven also.”)，然后提交，这时commit时间轴就会长出来一个新的节点，同时dev指针指向最新commit。 

这个时候master指针是指向上一个ccommit的，而且还没有做过任何修改，此时如果将dev分支合并到master分支，就是直接将master指针指向最新的一个commit而已，接着将HEAD指针重新指向master指针。画图说明。 

用git checkout master，可以切换回master分支，这时会看到上一次commit的代码，因为commit还指向上一个commit。 

用git merge dev，将dev分支合并到master分支，这个时候，master指针指向最新commit，HEAD重新指向master指针。 

此时代码就是最新一次commit的代码了。 

上面这种合并方式，实际上master是没有过任何修改的，合并dev到master，只不过相当于让master分支快进到dev分支指向的commit而已，就是fast-forward模式的merge。 

合并完分支之后，可以删除dev分支，此时相当于就是删除dev这个指针而已。画图说明。 

git branch -d dev，可以删除dev分支，此时就会删除dev这个指针了。 

（2）解决分支冲突 

如果在拉取了两个分支，然后两个分支并行开发，接着其中一个分支先合并到了master，相当于master分支的内容已经修改了，接着另外一个分支再合并到master，此时可能会出现，两个分支对同一行代码都做了修改，就会出现代码冲突问题！ 

git checkout -b feature1

git checkout -b feature2 

创建两个功能分支，如上面的命令 

在feature1分支将最后一行修改为System.out.println(“I like spark......”)，提交代码

在feature2分支将最后一行修改为System.out.println(“I like storm......”)，提交代码 

这个时候的情况，在之前的最新commit，拉出来了两个并行的commit，同时两个分支的指针分别指向对应的commit，画图说明。 

接着切换到master分支，将feature1先合并到master分支，那么此时master指针就会指向feature1指向的那个commit，同时HEAD指向master指针。画图说明。 

接着继续将feature2合并到master分支，此时，因为master分支是无法执行fast-forward快进操作的，因为master也是有修改的，此时只能执行3-way merge。而且执行git merge feature2命令后，会提示出现了confiict冲突，让我们先解决冲突。 

此时我们可以查看HelloWorld.java代码，发现其中最后一行变成了这个样子 

<<<<<<< HEAD

System.out.println(“I like spark......”);

=======

System.out.println(“I liike storm......”);

\>>>>>>> feature2 

上面的意思，就是说master分支和feature2分支出现了冲突，第一行是HEAD代码，因为HEAD目前指向master，也就是master的代码；第二行是feature2的代码。 

这个时候，我们可以将冲突的内容删除掉，保留正常内容即可。 

接着提交HelloWorld.java代码，就解决了冲突并且提交了代码，同时完成了feature2到master的合并。此时会自动长出来一个新的commit节点，这个commit就是解决了master和feature2冲突并且合并之后的commit。同时master指向最新的这个commit，HEAD还是指向master。画图说明。 

最后删除feature1和feature2两个分支，画图说明。

远程分支操作内幕原理

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1207.png) 

### 13、功能分支工作流实战以及深度图解分支内幕原理（三）

5、分支合并冲突 

常见于这样的一种场景，就是张三提交了一块代码，push到了远程仓库 

李四此时要push发现不行，要先git pull，会发现说跟张三的master分支代码合并的时候，很容易出现合并冲突 

张三修改了一个代码的一行，然后李四也修改了这一行，会发现，俩人修改了同一行，此时应该听谁的呢？ 

所以说，这个时候就需要进行冲突的解决 

李四和张三俩哥儿们会商量一下，看一下这个块儿代码，是怎么回事，为什么出现了冲突 

一般来说尽量避免冲突，让不同的人负责不同的独立的模块对应的工程，大家修改的代码都是不一样的，从根本上避免代码出现冲突

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1301.gif)



![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1302.gif)

出现冲突的代码，会出现上面这样乱七八糟的情况 

但是大家不要着急，我们是可以看懂的 

<<<<<<< HEAD

  System.out.println("I'm Lisi, hello wolrd......");

======

  System.out.println("I'am Zhangsan, haha......");

\>>>>>> 7670872121112446521ef12f5050e2ba825ce994 

这里的HEAD指的意思是说，李四本地的HEAD指针指向的分支对应的commit中的代码 

7670872121112446521ef12f5050e2ba825ce994，指的是张三修改后提交的commit对应的hash值 

所以这里就要解决冲突了啊，张三和李四一合计，张三说，哥儿们，行，那要不就用你的代码吧 

就是要删除张三的代码，然后删除特殊符号，即可解决冲突 

你每次解决冲突之后，会形成一个新的版本的代码，就是合并了两个commit并且解决冲突之后的代码，就是一个新的commit 

（1）分支原理再次回顾 

分支管理，是非常重要的 

因为一般来说，都是多个人在协作开发，每个人可能做一个功能，这个时候每人都会并行开发一块代码。那么就需要为每个功能创建一个代码分支，那个分支有完整而且独立的代码，每个人在自己的分支代码上开发，不影响任何其他人。最后都开发完了，再将几个分支的代码合并在一起。 

每个代码分支，都会有多次提交，然后每个代码分支都会处于最新一次提交的代码，这个分支的多次commit，就会形成一条时间轴一样的东西。 

默认情况下，git给我们创建一个master分支，每次提交都会提交到master分支。 

而master分支是指向最新的一次commit的，然后HEAD指针是指向master分支的，是这么一个关系。每次提交到master，master分支就会新增一个commit，然后master是指向最新一次commit的，同时HEAD又始终指向master。画图说明。 

此时如果我们从master分支创建一个其他的分支出来，那么此时git会创建一个指针，叫做dev，这个dev是指向master分支最新一次commit的，然后HEAD指针会指向dev指针。此时如果我们再提交代码，就是提交到dev分支了，此时commit时间轴会延长一个节点，同时dev指针指向最新的commit，HEAD依然指向dev。画图说明。 

git checkout -b dev，这个命令就是创建dev分支，dev指针指向最新一个commit，同时HEAD指针指向dev指针 

git checkout -b，相当于两个命令，git branch dev，git checkout dev，先创建分支，再切换到分支 

接着用git branch命令，可以查看当前的所有分支以及我们目前所处的分支 

接着我们可以在dev分支上修改代码，在HelloWorld.java中增加一行：System.out.println(“I like maven also.”)，然后提交，这时commit时间轴就会长出来一个新的节点，同时dev指针指向最新commit。 

这个时候master指针是指向上一个ccommit的，而且还没有做过任何修改，此时如果将dev分支合并到master分支，就是直接将master指针指向最新的一个commit而已，接着将HEAD指针重新指向master指针。画图说明。 

用git checkout master，可以切换回master分支，这时会看到上一次commit的代码，因为commit还指向上一个commit。 

用git merge dev，将dev分支合并到master分支，这个时候，master指针指向最新commit，HEAD重新指向master指针。 

此时代码就是最新一次commit的代码了。 

上面这种合并方式，实际上master是没有过任何修改的，合并dev到master，只不过相当于让master分支快进到dev分支指向的commit而已，就是fast-forward模式的merge。 

合并完分支之后，可以删除dev分支，此时相当于就是删除dev这个指针而已。画图说明。 

git branch -d dev，可以删除dev分支，此时就会删除dev这个指针了。 

（2）解决分支冲突 

如果在拉取了两个分支，然后两个分支并行开发，接着其中一个分支先合并到了master，相当于master分支的内容已经修改了，接着另外一个分支再合并到master，此时可能会出现，两个分支对同一行代码都做了修改，就会出现代码冲突问题！ 

git checkout -b feature1

git checkout -b feature2 

创建两个功能分支，如上面的命令 

在feature1分支将最后一行修改为System.out.println(“I like spark......”)，提交代码

在feature2分支将最后一行修改为System.out.println(“I like storm......”)，提交代码 

这个时候的情况，在之前的最新commit，拉出来了两个并行的commit，同时两个分支的指针分别指向对应的commit，画图说明。 

接着切换到master分支，将feature1先合并到master分支，那么此时master指针就会指向feature1指向的那个commit，同时HEAD指向master指针。画图说明。 

接着继续将feature2合并到master分支，此时，因为master分支是无法执行fast-forward快进操作的，因为master也是有修改的，此时只能执行3-way merge。而且执行git merge feature2命令后，会提示出现了confiict冲突，让我们先解决冲突。 

此时我们可以查看HelloWorld.java代码，发现其中最后一行变成了这个样子 

<<<<<<< HEAD

System.out.println(“I like spark......”);

=======

System.out.println(“I liike storm......”);

\>>>>>>> feature2 

上面的意思，就是说master分支和feature2分支出现了冲突，第一行是HEAD代码，因为HEAD目前指向master，也就是master的代码；第二行是feature2的代码。 

这个时候，我们可以将冲突的内容删除掉，保留正常内容即可。 

接着提交HelloWorld.java代码，就解决了冲突并且提交了代码，同时完成了feature2到master的合并。此时会自动长出来一个新的commit节点，这个commit就是解决了master和feature2冲突并且合并之后的commit。同时master指向最新的这个commit，HEAD还是指向master。画图说明。 

最后删除feature1和feature2两个分支，画图说明。

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1303.png) 

###  14、基于linux安装部署企业私有的GitLab服务器

之前我们是基于比较low的方式手工搭建了一个git服务器，然后在上面放了一个git仓库，而且企业级里面一般不会这么干1627098499 

实际上，我们可能会需要在git服务器上用可视化界面的方式去查看所有的提交，分支，以及代码，包括需要在git服务器上去进行分支的合并，而不是在本地去执行分支合并，分支合并之前，我们可能需要先执行code review，可以在git服务器上可以看到本次修改了哪些代码？ 

包括，还有很多其他的事情可以做，然后一些代码的bug缺陷管理和追踪 

真正在企业里，都是用专用的git服务器，gitlab，其他的一些git管理软件 

硬件准备，给一台2核4G内存的虚拟机，因此需要使用64位的centos 

1、Gitlab安装 

（1）第一步：在系统防火墙上开启允许ssh和http访问

```
yum install -y curl policycoreutils-python openssh-server cronie
lokkit -s http -s ssh 
```

（2）第二步：安装postfix来支持gitlab发送邮件 

```
yum install -y postfix
service postfix start
chkconfig postfix on 
```

（3）第三步：安装gitlab 

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
EXTERNAL_URL="http://192.168.31.80" yum -y install gitlab-ee 
```

此时会自动安装和配置gitlab，同时在指定的url启动gitlab 

（4）第四步：查看gitlab状态 

gitlab-ctl status 

（4）第四步：按照上面指定的url访问gitlab 

重新设置密码，然后使用root和自己设置的密码来访问gitlab 

2、使用Gitlab 

（1）以管理员身份登录进去之后，可以创建项目了，但是创建之前，可以先创建组和用户，然后创建项目的时候，需要将项目选择归属到组或者用户。一般一个组就是一个大的部门，一般选择为private私有的。 

实践经验 

比如说，你的公司比较大，有一个团队是基础架构团队，做的都是kv，nosql，缓存，开源项目的二次开发、运维和管理；还有一个团队是做的面向终端用户的，可能做的就是电商业务系统 

你的不同技术团队，开发的项目是不同的，可以一个group作为一个团队 

每个Group里面有对应的多个用户 

（2）进入项目，在settings->members中，还可以手动设置除了组之外的其他人有权限访问这个项目 

（3）将自己本地生成的ssh key公钥，添加到gitlab中去。使用开发人员自己的账号登录gitlab，然后选择右上角的profile setting -> ssh keys，将自己的ssh key加入进去。 

（4）将oa项目的代码修改成远程仓库为gitlab上的oa-parent 

需要在gitlab上，去掉对oa-parent项目的一个master分支的强制保护 

默认情况下，gitlab做的非常好，就是将master分支给保护住了，不让任何研发人员直接push代码到master分支的，要求必须采用pull request方式，在gitlab上提交merge的请求，通过管理员的code review 

如果一个成员是master权限，那么他就可以将代码直接push到master，同时只有master权限的可以去review pull request 

git remote remove origin

重新再次：git remote add origin gitlab项目地址 

张三和李四的本地仓库都做一样的事情 

张三，执行：git push -u origin master，再次将本地仓库的代码推送到gitlab上去 

查查看gitlab上的仓库，就已经有代码了 

（5）此时试着在张三本地仓库修改代码，推送到远程仓库，然后李四拉取代码，都走通，就ok了 

正常来说，如果李四就是第一次从gitlab拉取代码下来，那么其实是会自动将本地master分支和origin/master关联在一起的 

但是我们刚才的情况，是都更改了一下remote origin 

所以这里要重新手动将本地master分支和远程origin/master分支做一下关联：git branch --set-upstream-to=origin/master master 

3、维护gitlab 

启动gitlab：gitlab-ctl start

停止gitlab：gitlab-ctl stop

重启gitlab：gitlab-ctl restart 

gitlab日志：/var/log/gitlab 

查看gitlab日志：gitlab-ctl tail

查看gitlab对应的Nginx访问日志：gitlab-ctl tail nginx/gitlab_access.log

查看gitlab对应的数据库postgre-sql的日志：gitlab-ctl tail postgresql 

gitlab数据存放目录：/var/opt/gitlab/git-data 

gitlab是一种开源的管理软件，实际上做到这里为止的话，基本上你就可以玩儿起来了 

授人以鱼不如授人以渔。。。。我不太想给大家讲里面的所有的功能，你可以自己去探索，而且可以自己去看官方文档 

gitlab使用文档：http://docs.gitlab.com/ce/

### 15、面向极小项目的2人小团队集中式工作流实战

上一讲开始，我们已经搭建好了一款企业级的gitlab服务器，上面也建好了部门和员工，还有项目 

这一讲开始，我们就基于gitlab服务器，来实战企业中针对不同规模的团队，或者不同类型的项目，实战演练不同类型的git工作流 

功能分支工作流：5人以内的小项目，面向内部的那种需求不是很频繁的内部系统 

集中式工作流：2人以内的项目，1人单独维护的一个项目 

某2个同学，负责维护了一个小型的工具类的系统，比如发短信/邮件/消息的这么一个工具；某2个同学，负责维护大数据计算作业类的工程，比如说spark作业，spark就是一个工程，然后在里面你要去维护一套代码，但是这套代码的话呢就是实现一套不同类型的计算任务 

比如spark工程中，有的作业是用来分析用户行为的；有的作业是用来分析用户活跃度的 

不需要分支，直接每个人都是这个项目的master，都可以直接push代码，需求更新频率也很低，不是经常要修改代码的 

两个人都直接基于master分支去做开发，也不涉及什么code review，pull request 

实战一下集中式工作流： 

（1）张三和李四都在master分支上 

（2）张三先修改一下代码，然后直接push到gitlab上去 

（3）李四也修改代码，同时跟张三修改了同一行代码，跟张三会出现冲突，此时push失败，要求先pull，pull之后会要求解决冲突 

（4）解决冲突，提交代码，再次push 

基于gitlab的集中式工作流，就是这么玩儿的，说实话，因为就一两个人开发，所以不需要使用太多的gitlab的功能，就是基于一个master分支开发，一条路走到黑 

梳理和总结 

1、不管是谁，不管在哪里，不管是哪个分支，你做提交，其实就是在自己本地的提交历史树上加一个新的commit，然后你当前的分支就指向最新的那个commit而已 

2、你切换分支，新建分支，只不过是在切换指向的commit而已；因为不同的分支是指向不同的commit的，对应的是不同版本的代码；新建分支只不过是多一个指针而已，指向当前的这个commit 

3、在本地基于各种分支完成了代码开发，每个分支指向对应的一个commit之后，完成了自己本地的提交历史树的修改了，就可以将本地的提交历史树，推送到远程仓库，跟远程仓库的提交历史树进行合并 

4、此时远程仓库会和你的本地的提交历史树保持一致 

5、其他研发人员跟你一样，同时可以从远程仓库拉取代码，也就是拉取提交历史树，跟自己本地的提交历史树进行合并，可能会出现代码冲突。你和其他人都对同一个分支进行修改，此时你们俩的提交历史是不一样的。对你来说，一个分支是指向一个commit，对他来说，同一个分支是指向了不同的commit。此时就需要对这个分支指向的两个commit进行代码合并，可能需要解决冲突。合并之后会出现一个新的commit，就是合并了你们两个人的代码的一个commit，然后分支指向那个commit。

集中式工作流

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1501.png)

集中式工作流提交历史变迁原理

 ![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1502.png)

###  16、基于rebase优化集中式工作流的提交历史

这一讲，我就讲解rebase这个东西，为什么放这儿讲解？ 

这里的话，我说几句闲话 

上一次有同学说，看完maven之后，问，能不能讲解git之后，直接讲解一下jenkins 

不行啊，因为的授课风格和授课理念，未来应该全部是实战驱动，场景驱动，需求驱动，去学习每一块技术 

git，从需求出发，就是说，你需要进行版本控制了，才需要去使用git，然后上来头8讲，都在讲解版本控制这个事儿，包括怎么提交和记录版本，怎么进行版本控制 

从分支那块儿开始，都是出于基于git，在不同类型的项目，不同规模的团队的场景下，用什么样的既有分支的协作工作流去进行开发 

我没有死板板的给你讲解，branch，一共20个命令 

直接就是实际的功能分支工作流场景出发，一步一步学习branch命令，功能分支工作流场景实践完了，你的branch命令就全都掌握了 

前8讲，git如何做版本控制这个本质玩儿清楚了，git add，git commit，git status，git log，这些，命令自然而然就清楚是怎么回事了 

我不是说，git add，git commit，git status，git branch，git remote 

git完事儿了，怎么就直接讲jenkins了呢？？？很突兀，莫名奇妙就开始讲解jenkins了，死记硬背，什么功能 

我要直接就是在后面有一个大的阶段，第二个大阶段，微服务架构阶段，jenkins融入到那个阶段里面去讲解，微服务场景下，jenkins是怎么去玩儿的 

rebase当做一个很高深的技能，放在最后几讲，git的深入内幕命令，rebase，高深，高手 

rebase在实际工作场景中很少用，他的适用场景也有限，但是不是说rebase就没用，也有用，是有其适用的场景的 

集中式工作流这个场景结合起来去讲解rebase，一方面是讲解你的这个rebase的原理，一方面是讲解rebase在集中式工作流场景下，使用它的作用是什么呢？ 

在刚才那种普通的集中式工作流开发流程下，会有一个问题，就是每次如果某个人push了代码到master，然后另外一个哥儿们也要push会失败 

此时那个哥儿们就要git pull拉取和合并master分支的代码 

坑爹的事情来了，每次都这样合并的话，会导致你最后发现看到这个项目，它的提交历史就是各种分叉，各种合并，提交历史感觉很难看 

实际上来说，对于一个普通的集中式工作流开发的小项目，是不需要这样分叉的提交历史的，我们希望的是那种很平滑的，就一条线一样的提交历史，看起来会比较清爽 

要优化集中式工作流的提交历史成为一条线，看起来非常的清晰和清爽，此时就要使用rebase命令 

git pull --rebase 

rebase：变基，就是改变commit之前依赖的基础commit 

通过git pull --rebase，执行变基式的合并，改变commit历史，看起来提交历史就是一条直线，张三先提交，李四再提交，李四再提交，张三再提交 

集中式工作流的项目，看起来比较清爽 

比如说我们有一个master分支，还有另外一个分支比如feature/001 

也可以是合并的时候，采取git rebase feature/001来合并 

你就全部都采取git pull --rebase来跟其他人对master分支的修改进行合并，就能保持项目的提交历史就是一条直线

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\1601.png)

### 17、面向版本稳定迭代项目的中小型团队的GitFlow工作流实战

非常经典的GitFlow工作流 

这个工作流，我其实以前也用的挺多的，版本稳定迭代的中小型项目，中大型项目，版本稳定迭代 

1、GitFlow工作流是什么 

（1）项目刚开始建立，就两个分支，master分支和develop分支，指向的commit是一样的，代码是一样的 

（2）开始做第一个版本，v1.0，涉及到3个功能，3个码农，每个人开发一个功能，每个人从develop分支拉一个feature分支出来 

（3）张三，李四，王五，分别在自己的feature分支上，吭哧，吭哧，吭哧，埋头开发，每个人自己写自己的代码，写单元测试，本地冒烟测试，这个过程中，肯定是每天都会提交一部分代码，可能就是涉及到多个commit。gitlab，为了保证程序员本地的代码不能丢失，每天都完成一部分代码，然后就要将那个分支的代码推送到gitlab去保存。 

（4）一段时间之后，李四、张三、王五分别写完了代码，做完了自己本地该做的测试，就开始要做集成测试了。但是比较传统的方式，就是每个人先吭哧吭哧自己玩儿，写完代码之后，可能都好多天过去了，然后开始集成，互联网公司，不叫集成，联调测试 

（5）每个人都将自己的代码合并到develop分支上去，以develop分支作为基础进行集成测试，此时就可以三人关小黑屋，传统开发，没有用敏捷，持续集成，没有自动化测试。公司里弄一个会议室，三个人关在小黑屋，开始执行各种各样的集成测试用例，3个人的代码集成在一起，看看整个系统的流程能不能跑通 

（6）此时开始进行QA测试，QA妹子开始介入，90%都是女孩儿，搞一个这个v1.0版本对应的release分支，从develop分支拉出来，一边测试一边修复bug，QA测试结束，此时可以进行预发布测试 

（7）QA测试都结束了，功能，性能，压力，可用性，稳定性，易用性，进入预发布环节，要将release分支的代码合并到master分支 

（8）此时用master分支的代码在预发布环境，模拟线上的环境，进行测试和验证，最后的验证。互联网公司，可以从线上拷贝部分线上流量，观察系统是否正常运作。同时QA和PM，一起进行最后的验收和验证 

（9）直接用master分支的代码进行v1.0版本的上线，预发布测试完之后，马上就要上线了。先干一件事情，就是给此时此刻的master分支打一个tag，标签，v1.0.0。作为v1.0.0可以上线生产使用的代码的一个快照版本。以后如果要回退代码的话，很方便，直接使用v1.0.0 tag对应的代码即可。 

（10）上线之后发现有bug，此时需要从master分支拉一个bugfix分支下来，快速进行bug复现，修复，合并到master和develop两个分支。 

（11）每次master代码上线之前，都必须对master打一个标签，v1.0，v1.1。tag，标签，是什么？其实简单来说，就是指向master指向的那个commit的一个指针而已，tag指针是不能移动。说明这个commit就是一个可以稳定的可以上线的某个版本的代码。 

（12）feature分支用完了之后，是要删除掉的；release分支测试完了之后，也是要删除掉的；bugfix分支用完了之后，也是要删除掉的；唯一长期保存的只有master和develop两个分支，一个用于持续集成，一个用于稳定上线 

2、GitFlow工作流+GitLab的实战 

（1）首先要先准备好master和develop两个稳定分支，可以通过命令行去做，为了让大家学习git命令行的操作，其实真正在公司里，我们创建分支，都不是研发人员在本地自己创建的，都是在gitlab上创建分支的 

在本地创建develop分支，然后推送到远程仓库去 

git checkout -b develop，基于master创建了一个develop分支，切换到了develop分支 

git push -u origin develop，将本地的develop分支推送到远程仓库，同时在远程仓库建立一个develop分支，将本地develop分支和远程develop分支关联起来 

git branch -vv，看一下本地分支和远程分支的对应关系 

（2）模拟张三和李四两个人来做功能的开发，先做第一个v1.0.0版本，每个人拉一个feature分支，从develop拉的feature分支，分别是feature/001和feature/002，基于gitlab去创建 

（3）张三在本地将远程的feature/001分支拉取下来，会得到一个origin/feature/001，然后他需要在本地建立一个本地分支，叫做feature/001，将两者关联起来 

先执行：git fetch origin，可以得到两个最新创建的分支

再执行：git checkout -b feature/001 origin/feature/001，建立本地分支feature/001，跟踪远程的origin/feature/001 

李四如法炮制，在本地搞一个feature/002，同时需要搞一个develop分支 

（4）张三和李四在本地对feature001和feature002都开发完了，假设做完了单元测试，本地的简单的功能测试，冒烟测试，然后开始进行集成测试 

（5）张三和李四需要将feature/001和feature/002都merge到develop分支上去，注意，此时不再是在本地执行了，需要将feature/001和feature/002的代码都push到gitlab，然后在gitlab上提交merge request，发起code review，让级别更高的同事，或者是其他同事看一下你修改的代码有没有问题，进行代码走读，代码审查，code review 

（6）然后在develop分支进行集成测试 

（7）基于develop分支拉一个release/v1.0.0分支出来，基于这个分支开始测试，然后各种修复bug 

（8）然后将release/v1.0.0合并到master分支 

（9）对master分支打一个标签，然后master代码就可以上线了 

（10）上线以后，可能会发现张三负责的代码有bug 

3、GitFlow工作流适用的场景是什么 

适合的就是版本稳定迭代的项目，一个版本，接一个版本，接一个版本 

面向企业内部的系统或者项目，OA系统，CRM系统，ERP系统，或者面向外部的系统，比如说电商系统，但是系统已经成熟稳定了，版本稳定迭代 

4、GtiFlow工作流的不足之处在于什么 

我们之前做很多项目，刚开始都是遵照GitFlow去走的，但是如果是做一些互联网公司里面，面向外部的重要的业务系统，业务发展的过程中，快速迭代，同时5个版本，8个版本，10个版本，多达几十号人频繁的拉出多个版本并行开发 

develop分支会处于一个极其不稳定的状态 

而且不同版本的测试和开发进度不一样的 

比如v1.0已经集成测试完了，要进入QA测试了，但是此时develop分支还混合了v1.1版本的多个代码，你如果此时直接基于develop分支拉一个release/1.0分支，会把develop分支里面的v1.1的代码也带进去

 GitFlow工作流

 ![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\GitFlow工作流.png)

GitFlow工作流提交历史变迁原理

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\GitFlow工作流提交历史变迁原理.png) 

###  18、互联网公司多版本频繁并行场景下的工作流实战

如果多个版本频繁并行，此时完全参照GitFlow是不现实的 

develop分支里会混合多个版本的代码，同时在进行集成测试，如果一个版本先测试完，要先进入QA测试环节，是不可以直接基于develop分支去拉release分支的，因为release分支会混合多个版本的代码一块儿测试 

基于GitFlow工作流去一点点改进 

整个依赖的稳定的和基准的分支只有一个，就是master分支，全部以master分支为基准和基础 

（1）比如说启动一个版本 

v1.0，涉及3个功能，投入了3个RD（resourch developer，研发工程师），直接从master分支拉3个feature分支下来，不是从develop分支拉了，也没有develop分支了； 

同时要做一个v1.1，涉及5个功能，投入了5个RD去开发，直接从master分支拉5个feature下来，每个人就基于自己的feature去开发，搞 

同时的话呢，每个版本，刚开始启动，除了拉一堆feature分支，还需要同时从master分支拉一个develop分支下来，专门用于这个版本的集成测试 

（2）一个版本ready之后，要进入集成测试了 

很多时候，v1.1.0这样的版本，会比v1.0.0版本先上线 

经历过很多个大型互联网公司不同时期的项目，稳定时期的项目，我也做过 

（1）微型项目，集中式工作流

（2）小型项目，功能分支工作流

（3）中小型，中大型项目，维护，版本迭代缓慢，基于控制，一个版本一个版本的迭代，GitFlow工作流 

快速发展的业务和项目，不断的招人，不断的进新人，不断的开启各种版本，经常出现v1.5.3先上线了，但是v1.2.0还没上线 

基准代码只有master 

feature和develop都从master拉取，以master为准 

每个版本的RD各自迭代，各自独立开发，独立测试，加速开发和测试的进度 

直接跟master分支拉出来的staging分支去合并，release分支跟master分支的代码，以staging分支为基础进行合并，到这一步才进行多个版本之间的代码集成 

到这一步，master代码一定是稳定的，即使master分支混合了别的版本的代码，但是代码基本上都已经趋向于稳定了，所以此时进行多个版本的代码集成，风险是比较低的 

staging代码，回归测试，QA仔细回归一遍，要把集成在一起的多个版本的代码对应的功能，全部回归测试，确保每个版本的代码对应的功能都正常运行 

staging代码和master分支合并在一起，打tag，准备上线，这个版本就实现了上线 

如果在前期，develop分支就开始进行跨版本，跨团队的代码集成，就是一场噩梦

改进后的GitFlow工作流

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\改进后的GitFlow工作流.png) 

### 19、Git高阶实战技巧：比较不同分支之间差了哪些代码改动

就是上一讲结束之后，其实日常工作中，我们最常用的git这块的技术和工作流程，包括背后的原理，就全部结束了

写代码，提交代码，推送代码到远程仓库保存一下

多人协作的流程，搭配上我们的企业中的软件开发流程

背后的原理：三个区域，提交历史树

但是。。。git还远远不止这些东西啊。。。

尤其是说，在日常的开发过程中，可能我们不一定会每天都用到部分高级技巧，但是在你的整个项目发展的生命周期过程中，很可能遇到一些特殊得情况，此时是需要使用git的一些高阶技巧，来辅助我们完整一些实际工作中遇到的问题

从19讲一直到32讲，都是git的高阶实战技巧，我讲解的时候，会做到一个的标准，绝对不会说是一个一个git命令和语法去讲解，那样的话太枯燥了，你都不知道怎么用

我在讲解git高阶实战技巧的时候，一定是说，我过往的某个实际工作场景中，真正遇到过的问题，作为一个企业实战场景引出来，接着来学习某个git高阶技巧如何来解决这个问题，而不是会像其他的一些课程或者讲师那样，一个命令一个命令的去讲解

=================================================================================

比如我们在用reset回退到某个版本的时候，可能会使用commit的完整的40位的hash值来指定某个commit，但是实际上，我们是有更好的一些办法来选择一个或者多个commit。

版本的回退和前进，git reset --hard commit标识符

当时我们用的是完整的commit标识符，40位的SHA-1 hash值，但是在实际工作中，我们一般是不会用git的40位的SHA-1 hash值

1、短SHA-1值

我们不一定就是要用40位完整的SHA-1 hash值来选择一个commit，也可以只是提供hash值的前几位就可以了，至少要4位以上，同时在git仓库中就这前几位可以唯一定位一个commit就ok

就是说，我们如果提供hash值的前7位，git会自动唯一定位出来这个前7位对应的是哪个commit

实验一把

先用下面的命令查看最近几次commit：git log

然后再看一下某个commit的具体信息：git show 前7位hash置

所以正是因为这个，我们一般用git log，都会带上一个--abbrev-commit，再加上--oneline，--graph的选项，然后自动就会显示每个commit的短hash值，默认就是7位的

2、使用分支的指针指代commit

背后的原理大家都很清楚了，git最重要的原理

（1）三个区域（三棵树）：工作区，暂存区，仓库

（2）commit树+分支指针：无论在哪个分支上做开发和提交，都是在这个项目公共的一个commit树上在添加不同的commit，形成一颗越来越长的树。分支只是指向某个commit的一个指针而已。然后每个commit就代表了那个时刻下，项目的一个完整的历史快照版本。

因为分支实际上就是一些指针，所以分支是指向commit的，也可以直接用分支名称来指代它目前指向的那个commit

比如使用命令：git show master，就可以查看master分支指向的那个commit

3、git reflog

git log和git reflog的区别是啥？？？？

git log，是你当前的分支指向的commit之前的所有commit会显示出来，你的分支指向的那个commit之后的commit是不会显示的

git reflog，是会显示最近几个月，完整的一个commit历史的

所以说，在当前分支上，如果要回退到过去的某个历史版本的话，那么此时应该是用git log就可以

但是如果要回到未来的某个版本，是用git reflog

git会自动记录和追踪过去几个月内，你的HEAD指针和分支指针移动过的每个commit

用git reflog命令就能查看完整的一份commit记录，而使用git log就只能查看当前分支指向的那个commit之前的commit记录

reflog是每个研发人员自己本地的电脑上的记录，跟git log显示出来的提交历史不同，不会同步到远程仓库上去的

4、选择历史版本的特殊语法

git show HEAD^
git show HEAD~2

HEAD，指针，指向的是当前的那个分支，当前的那个分支又是指向某个commit

HEAD，就可以指代当前你所处的那个commit对应的版本

HEAD^，就是说，HEAD对应的commit的上一个commit，HEAD^^，上两个commit，HEAD^^^，上三个commit

HEAD~5，就是说HEAD之前的第5个commit

5、commit与分支

实际工作场景中经常会用到的一个技巧，比较两个分支之间差了哪些commit还没有合并？？

回答一个问题：我当前开发的这个feature/001分支，有哪些commit还没有被合并到master分支？

我要看一下，当前开发的这个feature分支，有哪些commit还没有被合并到master分支中去？如果知道了以后，心里就有数了，大概知道自己跟master分支之间差了多少代码。。。。

可能你开发一个feature分支，都很多天了，然后中间一度停下来，去做了另外一些事情，比如说其他的feature分支，或者是休假了。回来以后都忘了自己跟master分支之间差了哪些代码了，此时你就可以用这个方法，看一下

使用master..feature/001语法，就可以查看哪些commit是feature/001分支可以触达的，但是是master分支无法触达的，此时就可以看到feature/001分支做的那些改动哪些还没有合并到master分支里去

当前，我们处于feature/003这个分支，然后我们此时就要看一下说，我都忘了，我在这个分支上开发了多少代码，多少个commit了

git log --abbrev-commit master..feature/001 

同样可以看一下，feature/001分支比master分支落后了哪些commit，不是需要跟master保持一下同步

git show commit标识符

还可以看一下，master分支有哪些commit是feature/003分支所没有的，这个是个什么场景啊？这个场景很实用的，就是一般如果你开发一个feature分支时间过长，比如说都开发了几个礼拜了，master分支肯定代码都变化了好多了

这个时候你心里有点虚，你就担心说，会不会到时候我往master分支一合并，大量的代码冲突，心里有个底，会先看一下谁提交的，哪些commit在master上有，但是我自己的feature/003分支是么有的

git log --abbrev-commit feature/001..master

如果说你发现master上有好多的commit，别人提交了，你还没有，此时你应该做一下将master代码反过来merge到你的feature/003分支上去，让你的feature分支的代码跟master分支的代码保持一致，保持同步

此外，还可以看一下，哪些本地master分支的commit需要被推送到远程仓库中去，这个的意思就是说，如果你在本地的master上做了不少的提交和修改，可以看一看有哪些代码是还没有推送到远程仓库里去的

git log --abbrev-commit origin/master..HEAD

这个用法其实常见于不是master分支，就是如果你在本地开发一个feature分支，然后开发了几天，结果不小心忘了推送commit到远程仓库，某一天你想起来了，此时你就可以看一下，你忘了推送多少个commit到远程仓库的feature分支去

==================================================================

比较少用

还可以看一下，在两个feature分支中都有的commit，但是在master分支中还没有

git log --abbrev-commit feature/001 feature/002 --not master

同时也可以看一下哪些commit是存在于两个分支中的某一个，但是不是两个分支都有的

git log --abbrev-commit feature/001...feature/002

### 20、Git高阶实战技巧：查看不同版本之间的代码差异具体是什么

1、本地查看 

git diff：工作区和暂存区 

git diff --cached：暂存区和仓库 

git diff HEAD：工作区和仓库 

git diff feature/001 master：两个分支之间的差异 

git diff HEAD HEAD^：最近两次提交之间的差异 

我在工作中常用的两个技巧，什么时候要去对比不同版本的代码差异呢？上一讲其实重点是关注的比较粗的层面，差了多少个commit 

比如说我今天开发了一些代码，还没提交，或者是已经提交了，此时我希望看一下，确认一下今天跟昨天的代码之间的差异是什么？ 

就是我告诉大家，大家作为一个程序员，码农，我喜欢的是那种心细如发的码农，很细心的人 

不细心的人，很容易造成各种乱七八糟的bug 

今天开发了一些代码，回头看看，跟昨天的比较一下，看看我到底加了哪些代码，我新增的这些代码会不会有什么bug？ 

我的一个工作习惯，每天必看今天做出的代码差异，确保这些代码差异没什么大问题，仔细反复脑子了复盘几次，梳理几遍，想想这么写代码有没有问题，有没有其他更好的方式去写代码 

其实这个过程，相当于是你对今天的工作做一个复盘，相当于是你对自己今天的代码做一个code  review 

随便举几个例子，如何对自己的代码进行查漏补缺： 

（1）看看，注释有没有写全？每个方法，每个类，类里面的核心代码，注释有没有写全

（2）类命名，方法命名，有没有什么问题

（3）有没有什么方法，圈复杂度过高，圈复杂度，如果你的方法里面，if else里面嵌套了一个if else，再次嵌套if else，嵌套了十来层，圈复杂度过高，重构一下方法，有些if else需要抽取出来做成单独的方法，便于维护

（4）你写的代码扩展性怎么样，如果以后要扩展的话，好不好扩展，能不能用什么设计模式来重新设计一下？ 

今天和昨天写的代码的差异看一下 

（1）今天写的代码还没有做commit，此时的话，代码还停留在工作区中，此时用一个命令：git diff HEAD，将工作区中的代码和仓库中最近一次commit的代码做比较，给你显示出来差异 

（2）今天写的代码已经做额commit，此时可以用：git diff HEAD^ HEAD，比较commit到仓库中的代码，最近两个commit之间的代码差异 

2、gitlab上查看两个分支之间的差异 

还有别的场景 

（1）你现在在写一个feature分支，你想看一下feature分支和master分支之前的差异，git diff  master feature/001 

（2）这个过程同样是相当于你自己在做code review，就是说不一定每天都干，但是，最好是每隔一段时间，都仔细看一下你最近做的代码修改和master之间的差异，确保说你的代码没有很大的问题，对你写的代码仔细的去思考，有没有什么大问题

### 21、Git高阶实战技巧：将暂存区中的多个功能代码分成多次提交

有的时候我们可能会一下子往暂存区里放很多的内容，给大家举一个非常实际的例子 

通常来说，比如说我们要开发一个版本，v1.0，拆分成多个模块，每个模块有多个功能。如果你手下有几个弟兄的话，一般是怎么拆分任务呢？每个人分配一个或者多个模块，这个模块中的多个功能就由那个哥儿们去负责开发。 

在做项目管理，每周都是要做项目管理周报的，列出来每个人负责的内容还有时间排期，进度，风险预案，下周计划。我们都是将开发任务细化到每天的，比如下面的这个表格 

| 时间       | 开发人   | 模块     | 功能           |
| ---------- | -------- | -------- | -------------- |
| 2017.01.01 | 中华石杉 | 权限管理 | 用户授权       |
| 2017.01.02 | 张三     | 部门管理 | 部门的增删改查 |
| 2017.01.03 | 张三     | 员工管理 | 员工的增删改查 |

那么如果在未来微服务架构那块要讲解的持续集成环境下，其实是要求每个研发人员，每天都要将这一天开发好的内容完成单元测试之后就提交，先进行代码的持续集成，然后触发一次自动化的单元测试以及持续集成测试 

当然都是玩儿的持续集成+敏捷开发+自动化测试，效率 

所以规范化要求，都是每天开发好的代码就提交一次，作为一个commit的 

此时就有一个问题了 

如果某天张三家里到下午3点的时候有事儿，孩子发烧了，39度了。老婆说，赶紧回来，人有三急，人有N急。着急忙慌往家里赶，忘了提交今天写好的那个部门管理的功能，然后第二天过来接着继续写代码，写好了另外一个员工管理的功能，然后一下子git add --all .，将昨天和今天写好的代码一起都放入暂存区，然后一次性来一个酣畅淋漓的git commit+git push，就完事儿了 

此时。。。 

张三突然想到，不对啊，怎么一下子要提交这么多代码，一语惊醒梦中人！！！ 

在规范化的，高效率的项目管理中，排期是细化到每天的，每天都要完成一定的功能或者模块，每天写好的都是一块完整的可以运行单元测试的代码，develop分支，GitFlow工作流 

好像应该将这批代码分成两次提交，才能符号公司的规范啊！每天写好的功能，就一个commit！ 

为什么要这么干？因为公司里面代码的commit历史是很重要的，后续的话呢如果说线上有bug，需要定位bug的责任人！！！这个时候是需要用git对代码进行二分查找，找到那个bug是谁造成的！！ 

commit历史，漂亮而且清晰的，可以让每个人在任何时候都看的很清楚，整个项目代码的变迁 

想象一下，如果每个人基本都按照公司的规范，99%的情况，其实最后公司的项目的commit历史是很漂亮的，很有价值。基本你可以看到每个人每天都干了什么？还得结合一套规范的commit提交备注，今天开发了什么什么模块。 

形成今天的这一套复杂的系统的 

我现在给大家讲解的，很多我不知道大家之前有没有这些概念，但是这都是一个规范的大公司里的大项目的要做的事情，BAT，规范是很严格 

符合规范，才能让项目的质量更高，更好 

中小型的公司里面工作，可能没有对这些那么的注意，那么此时我也建议大家都好好听听这些工作里的实践经验，大公司里的工作方法和规范 

如果你以后去大公司面试通过，去工作，你会觉得大公司的规范，让你感觉似曾相识，而且我后面的话，在整个做项目的过程中，都会给大家引入很多我多年在大公司里工作，做核心项目积累的经验，项目管控上经验 

要让你的兄弟们去遵守的 

或者如果你把这一套经验和规范吸收了，学会了，那么以后你如果去一个小公司里R一个大项目，架构师，带一群小弟，就可以把这些经验和规范应用到小公司里去，小公司也可以做的高大上一点儿，规范一点，标准化一点，漂亮一点儿 

这个时候就要用下面的高阶实战技巧了，假设你不小心将多天的代码都提交到了暂存区里面去，此时可以将暂存区里的内容分成多个commit来提交

此时需要先执行一个命令，进入交互式模式：git add -i 

这个时候会给你列出来当前暂存区里所有的文件和内容，以及没加入暂存区的文件和内容 

还有一堆可以执行命令 

此时张三写好的那些代码都被之前的git add --all .放入了暂存区，还好还好，我们可以搞一搞，恢复一下 

此时根据各种指令以及选择文件，将暂存区中的员工管理功能对应的文件和代码，先unstage出来，从暂存区里挪出来 

2：update是放入暂存区，3：revert是从暂存区里挪出来 

通过这个技巧，就可以将暂存区里部分文件给挪出来，然后再执行commit操作，将代码分成多次来提交，每天写好的功能是一个commit，跟公司的规范保持统一 

通过刚才演示的这个技巧，一旦你不小心将多天的代码都放入了暂存区，可以通过git add -i进入一个交互模式，然后选择将部分文件从暂存区里挪出来 

分成多次来提交 

我接下来很多讲，都不是纯讲git命令，会从我实际在工作场景中，针对我们大公司里的规范，遇到的一些特殊场景和问题，是怎么来运用git这些技巧的 

18讲，基本涵盖了git核心的所有的内容了 

多人协作，4种工作流 

如果你不太重视一些规范什么的，基本就够了，18讲 

但是我还是希望大家能够把后面的几十讲学好，为什么呢？？？因为这是架构师课程，这个不是说一个普通的什么java培训课程。 

你未来的定位是架构师，如果你的背景学历不错，可以进类似BAT或者大公司的话，你的定位就应该是技术专家， 带一个小团队，完全可以适应和按照大公司的标准和规范去玩儿 

如果你是进入中小型公司去做架构师的话，技术总监，CTO都看着你呢 

你有没有干货 

你有没有什么东西，是与众不同的 

那所以如果你能拿出一套非常棒的开发流程和规范，包括为了支持这套流程和规范，在git上的使用技巧，你需要对你的team去进行培训，拿出一种大公司的范儿来，镇住他们 

别上来还是小公司的一套玩儿法，技术总监，CTO，什么特别的

### 22、Git高阶实战技巧：feature分支开发到一半时切换到bugfix分支

之前说过了，如果你开发一个feature分支，正常情况下，都是每天去开发完一块完整的东西，某个模块，或者是某个功能，同时做完单元测试后，然后才会做一次commit的 

但是如果今天你开发一个feature分支开发到一半的时候，接到一个紧急通知说，一般看，运营那边过来，互联网公司的话。比如说你做一个电商网站，app。如果有bug，是谁发现？一般来说可能都是线上在使用的用户突然发现系统或者app运行部正常了，比如说突然电商网站，怎么商品显示不出来呢？他们会打电话给客服投诉，客服会转到技术部，也可能会转到运营那儿去。 

比如像我现在做的这个产品，也是公司的核心项目之一。一般有bug，客服找运营，运营直接通过一个缺陷跟踪系统来，jira。运营同学，会在jira上提一个task，工单，里面会写上详细的一些bug的信息，包括什么时间，谁反馈，截图，如何复现。指派给一个负责的RD和一个对应的QA。 

RD会去看一下那个工单，尝试在本地先复现出来，然后去修复那个bug，会部署到预发布环境，让QA测试工程师同学，妹子，快速的回归一下，验证一下那个bug是否修复了。o了以后，就直接上线。 

赶紧修复一个线上的bug，此时怎么办？ 

GitFlow工作流，修复线上bug，需要从master分支拉一个bugfix分支出来的 

你手头正在开发feature分支，代码写到一半，没弄完呢 

如果你此时要切换分支，在git中，是会让你先提交当前分支的代码，然后再切换分支的 

你是把写到一半儿的代码给commit一下呢？还是怎么玩儿？把写到一半儿的代码就提交一个commit的话，就很坑爹，包含了一半的代码。 

当然是立即从master分支拉一个bugfix分支出来了，但是问题是，如果要从feature分支切换到bugfix分支，你就必须先提交feature的代码，做一次commit，然后才能切换到bugfix分支 

这就不太好了吧，feature分支开发到一半，就贸然提交一下，感觉就比较坑爹了，弄一个不伦不类的commit记录出来，违反公司规范 

此时，实际上就是要用到stash功能，将feature分支开发的代码暂存起来 

stash功能，其实就是将当前feature分支上对应的工作区的修改过的代码，全部暂存起来，然后将工作区的代码恢复到干净的状态，好像什么都没修改过一样 

接着此时你就可以切换到master分支，拉一个bugfix分支出来，接着的话修复bug，上线，bugfix分支合并到master分支和develop分支 

都搞完了之后，删除bugfix分支，同时在本地再次切换到feature分支，将之前stash起来的修改好的到吗恢复到现场，继续开发 

假设我们现在在feature分支上做了一些开发，修改了一些代码，此时需要立即切换到一个bugfix分支去修复bug 

此时执行一个命令：git stash 

现在执行git status看一下，会告诉你工作区没有任何修改的文件 

可以给大家说一下，除了代码都是瞎写的，用来演示的，但是git最重要的是工作流，我都是按照真实企业级的工作流去模拟的，我们大公司，线上核心系统，整套工作流程，结合gitlab，就是这么玩儿的 

看好，自己看完了视频以后，照着视频里的步骤，把这些流程都走一走，去体会，感触很深。后面我们正式开始做大电商系统，也完全是按照我们公司里的规范去走的 

然后我们就可以git checkout -b bugfix/002，切换到bugfix分支快速修复问题，修复完了之后，就会合并到master分支，同时删除bugfix分支 

接着我们切换回feature分支 

同时我们可以查看一下stash列表：git stash list 

执行命令，将最近一次stash给恢复回来：git stash apply 

也可以使用git stash apply stash@{0}，恢复到指定的一次stash 

此外，如果执行git stash的时候，有放入了暂存区的内容，在恢复的时候需要使用git stash apply --index 

也可以用git stash drop stash名称，手动删除掉某个stash 

此外还有一个有用的命令，就是默认情况下，git仅仅stash那些tracked bug modified文件，但是如果要同时stash那些untracked文件，需要使用： 

默认情况下，它只会将工作区中的修改的文件进行stash，modified状态。如果要将untracked和暂存区中的文件都进行stash。git stash --index --include-untracked，就是将暂存区中的内容，tracked和untracked的内容，全部进行stash暂存 

git stash apply --index，就是同时将工作区和暂存区中的内容进行恢复

### 23、Git高阶实战技巧：对本地不规范的提交历史进行修改和调整

这一讲的内容，相对来说，真的是git的一些高级功能了 

大前提，我们规定的是每天完成一块功能，然后每天都对这个功能完成单元测试，同时提交一次，形成一个commit 

但是问题是，我们每天在本地可能会遇到各种突然情况，然后会导致违背上面的那个提交规范，此时我们就需要对提交历史进行调整，来重新规范化我们本地的提交历史 

前提，本讲讲到的这些对commit的调整，全部是针对的是本地，那些commit还没有被push到远程仓库去，仅仅在本地执行了git commit命令 

1、对最近一次commit的备注或者代码进行修改 

（1）修复上一个提交的不规范的备注 

如果你刚刚执行了一个commit，结果发现，不好，我其实应该补充更多的备注，才能符合公司对commit备注这块的规范的，刚才写的太简单了 

没关系，可以修改一下。。。。。。前几讲，也给大家演示过了，就是修改上一次commit的提交备注 

git commit --amend 

直接就可以修改上一个commit的备注信息，补充更多的提交说明 

相当于是将上一个commit删除掉，然后基于上一个commit对应的代码重新构建一个commit object 

（2）对上一次commit加入几行遗漏的代码 

或者说，你刚执行了一个commit，结果扫了一眼代码，发现今天忘记修改几行代码了，需要修改之后补充到上一个commit中去，没关系 

先修改你的代码，同时加入暂存区，然后再次执行： 

git commit --amend 

就会将你最新修改的代码跟上一次合并到一个commit中去 

但是这里要注意，git commit --amend是会修改commit的SHA-1 hash值的，所以如果一个commit已经push到了远程仓库，就不要再去修改了，否则的话会造成提交历史的一个混乱 

所以说，本讲针对的全部都是本地的提交历史 

2、对历史上的多个commit进行修改 

这个场景，常见于这样，你可能在本地连续几天都提交了几个commit了，但是一直都忘了push到远程仓库。此时突然有一天，你发现你需要将这个几天的commit都是一次性push到远程仓库，但是你看了一下，发现你3天前的某个commit的备注不规范，需要调整一下，此时你需要。。。 

git commit --amend，只能针对最近一次commit去修复和调整 

使用git rebase -i HEAD~3，就是调整最近3个commit 

同时要注意，如果你已经推送到了远程版本库的commit，不要修改和调整 

然后需要将你需要调整的commit之前的pick修改为edit，比如选择其中一个commit 

此时你需要对你要修改的commit，一个一个的进行修改，可以修改3天前的那个commit的备注，执行git commit --amend即可 

你如果要对前几天的某个commit加入更多的代码，可以仿照之前讲解过的那个先修改代码，加入暂存区，接着git commit --amnend，就可以加入代码。或者是直接执行git commit --amend，修改备注即可。 

修改完了一个commit之后，可以执行下面的命令，继续修改下一个commit 

结束之后，执行git rebase --continue 

最终会将你设置为edit的那几个commit都让你一个一个的修改 

但是我这里说下，对前几天的commit，一般是不加入代码的，除非是特例，比如说你发现你3天前些好的某个功能，忘了加一些非核心的代码，比如说日志代码，监控代码，那么你可以加入那些代码 

3、删除commit以及调整commit的顺序 

（1）删除commit是需要的，比如说你偶尔，不小心手贱，或者是一个应届生，不小心就加了一个空行，结果就搞了一个提交，此时需要将那个commit给删除掉 

（2）调整commit的顺序，这个就不太靠谱了 

接着上面的命令，可以对多个commit，删除你不想要的那个commit，同时也可以调整顺序，然后保存 

接着git直接会自动追溯到那几个commit的最近一个parent commit，重新按照你的顺序应用代码改动 

这个场景常见于说，你不小心进行了某次无意义的提交，举个例子，不小心加了一个测试代码，结果还提交了，其实那个commit是不需要的，此时可以用这个办法删除那个commit 

4、将多个commit合并为一个commit 

如果恰巧你就是今天不小心做了多次commit，干脆破罐子破摔吧，就做多次commit 

但是最后今天要push到远程仓库之前，可以将今天的多个commit合并在一起的 

就是在上面的那个界面，可以将多个commit前面的命令修改为squash，就会自动将这些commit合并为一个commit 

提示一下，如果要合并多个commit，那么一定要往前多选择一个commit 

5、将一个commit切分为多个commit 

还有一种情况，就是你不小心将3天修改的代码都合并成了一个commit了，此时木已成舟，没法用git add -i来多次commit了，咋办？ 

没事的，将这个commit拆分为多个commit就好了 

还是在上面那个界面，将要拆分开来的commit前面的指令，修改为edit 

然后你就会回到那个commit对应的版本，此时执行git reset HEAD^，此时会直接回退到之前的状态，然后此时可以执行多次git add和git commit，形成多个commit 

接着最后执行git rebase --continue结束这次commit拆分 

6、最后总结 

其实上面所有的场景，针对的都是那些大公司，对提交历史有一定严格规范和要求的公司 

如果你违反了那些规范的话，需要手动去调整你的提交历史的

### 24、Git高阶实战技巧：对本地刚做出的修改、暂存和提交进行撤回

这块针对的是在本地，如果你刚对工作区的代码进行了修改，或者将部分代码加入了暂存区，或者是提交了一个commit，此时你后悔了，那么如何吃后悔药 

1、先回顾一下git中的三个概念 

HEAD就是一个指向当前分支的一个指针 

index，也就是暂存区，其实就是将会形成下一次提交的文件 

工作区，其实就是我们的开发区 

刚开始工作区中会有一些文件 

然后我们执行git add命令可以将工作区中的文件加入到暂存区中 

接着我们执行git commit命令，可以将暂存区中的文件一次性提交到git仓库中，同时创建一个commit object指向一个tree object，tree object指向那些文件对应的blob object。 

然后master指针会指向那个commit object，HEAD指针指向master 

我们同样可以对文件再次修改，让一个文件变成第二个版本，然后再次git add和git commit，此时暂存里的文件会变成第二个版本，同时也会提交到仓库里创建一个新的commit object 

新的commit object会引用上一个commit object 

假设我们又修改了第三次，同时提交了，那么此时会有此三个commit object，同时引用第二个commit object，工作区和暂存区也都是第三个版本 

2、reset撤回操作 

（1）执行git reset --soft HEAD^，会有下面的影响 

1）在仓库中，将master分支指向上一个commit

2）暂存区和工作区还是停留在第三个版本，没有变化 

因此这个命令，相当于是撤回了git commit操作，因为git commit操作，就是将暂存区中的第三个版本放入仓库，同时创建一个新的commit object指向第三个版本，master指针也指向最新的commit object 

此时就相当于取消了刚执行完的git commit操作 

我们可以再次对暂存区加入更多的内容，然后再次执行git commit操作，此时再看一下效果，你会发现跟git commit --amend类似 

（2）执行git reset HEAD^，默认等同于git reset --mixed HEAD^ 

回到三个区域都是第三个版本的状态，执行git reset HEAD^ 

1）将master指针指向上一个commit

2）将master指针指向的commit对应的版本恢复到暂存区中 

此时暂存区中的文件版本会变成第二个版本 

这个命令，相当于是撤回了git commit和git add操作 

这里我们可以想象一下，如果用git reset HEAD，就可以实现将刚git add到暂存区中的文件版本回退到仓库中的上一个版本，相当于就是回退了git add操作 

（3）执行git reset --hard HEAD^ 

相当于是彻底撤回刚才的git commit，git add，以及工作区中的代码修改 

因为这个命令，会将指针指向上一个commit，同时暂存区里的内容变成上一个commit的版本，同时将上一个commit的内容恢复到工作区 

其实这个就是之前讲解过的，相当于是回退到历史上某一个版本了 

（4）梳理一下 

1）如果刚刚修改了工作区中的内容，但是没放入暂存区和仓库，此时要抛弃这些修改，怎么办？ 

这种情况是很常见的，一般见于什么呢？临时修改技术方案。。。。 

比如说，你现在要开发一个工作流模块，然后之前定的技术方案是基于jBPM 4.x去开发，然后呢，你就开始吭哧吭哧吭哧开始写代码，引入了jBPM 4.x的jar包 

写了几天，你的leader，架构师，反悔了，他说，我前两天没设计好，考虑不周到，要不换成用Activit去开发吧。。。 

你做了几天，发现jBPM 4.x的功能不能满足你的需要，临时换框架 

也可能是，做了几天以后，发现其他团队已经有一个现成的基础类库，可以直接基于他们的来封装 

如果说此时你修改的代码比较少的话，那么就用git checkout -- 文件名，就可以将这个文件的内容恢复到上一次提交的状态，完全抹掉你最近的修改 

但是如果你已经修改了很多代码了，此时你需要来个一次性的，强有力的恢复：git reset --hard HEAD 

git reset --hard HEAD 或者是 git checkout -- 文件名 

意思就是保持仓库和暂存区里的文件版本为HEAD最新版本，同时将上一次提交的HEAD版本写回工作区中，覆盖掉这次修改。 

2）如果修改了工作区中的内容，同时放入了暂存区，但是还没提交，此时要抛弃这些修改，怎么办？ 

（1）你希望将暂存区里加入进去的内容撤销掉，然后在工作区里面重新修改代码 

git reset HEAD = git reset --mixed HEAD，一样的命令，就是将仓库中HEAD指向的上一个commit，恢复到暂存区，同时恢复到工作区 

（2）你希望将暂存区和工作区里面的内容一次性抹掉：git reset --hard HEAD 

3）如果修改了工作区中的内容，放入了暂存区，还提交了，怎么办？ 

本来今天的代码你都提交了，但是你的leader说，这块代码不要了，此时怎么办？回退版本，直接回退到没做任何代码修改的那个版本 

git reset --hard HEAD^，git reset --hard commit标识符 

git reset --hard HEAD^，分支指针指向上一个commit，同时上一个commit的内容恢复到暂存区，也恢复到工作区 

git reset --hard v1_commit_标识符 

git reset --hard HEAD^^ 

（4）总结 

上面说的，同样都是针对本地的commit，一旦你已经push到了远程仓库，就不能这样了，否则会弄乱你的提交历史

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\reset命令内幕原理.png) 

### 25、Git高阶实战技巧：远程和本地同时撤回分支合并操作

1、在出现冲突的时候 

如果两个分支merge出现了冲突，临时打算取消注意，不想merge了，那么此时可以执行git merge --abort命令即可 

2、如果仅仅是在本地完成了merge后想要撤回 

很简单，直接执行git reset --hard HEAD^，将分支挪回merge之前的那个commit即可，就相当于没执行过merge 

3、如果是在推送了merge后的commit到远程仓库之后想要撤回 

我给大家说一个非常经典的实际的案例，工作中之前经常遇到的 

比如说，我们之前讲过，按照GitFlow工作流来走，最后是在release分支进行QA测试，测试通过之后，会合并到master分支，准备进行预发布测试和上线 

但是。。。。。 

不靠谱的队友，不靠谱的pm 

不靠谱的队友，你是跟其他团队协作一起上线的，此时你的仓库的代码都合并到了master，确认没问题了，服务A，依赖于其他团队开发的服务B，在不同的仓库里的两个项目；但是。。。你要一起上线的其他团队的同学，服务B，突然发现在QA环境，临时测试出来了一个大bug，此时pm沟通了一下，评估了一下风险，决定，今天取消上线，下周再上 

尴尬了 

你的代码都合并到master分支了。。。但是说不上线了，此时你是不能将不准备上线的代码留在master分支里的 

如果你有其他需求在最近一两天也要上线，也会合并到master分支，然后如果那个需求上线，master分支会把你的那个不准备上线的功能的代码，给带上线 

所以这种情况，是不能接受的 

常见于大公司，上百人协作的那种大项目，比如某巨头公司的广告系统，最核心的系统，牵扯到上百人，数百人一起在搞，架构升级 

如果碰到这样的情况，你是需要将本次之心的merge给撤回的，无论是本地还是远程，都要撤回 

但是如果push到了远程仓库以后，你是不能再本地reset一下，再push的，因为那样会搞乱你的提交历史的  

不上线了。。。撤回release/v1.2.0到master分支的合并 

需要在本地执行一个命令：git revert -m 1 HEAD 

git revert实现的效果，不是说指针往回挪动，而是说创建一个新的commit，对应的代码版本跟合并之前的那个commit是一模一样的 

-m 1的意思就是，恢复到当前这个分支合并之前的那个状态，此时会创建一个新的commit，这个新的commit对应的版本跟合并之前的commit是一模一样的 

这就完美撤回了merge操作，同时此时如果执行git push origin master，会让远程仓库的master分支也回退到merge之前的状态，但是也多了一个新的commit出来 

但是此时有一个问题，就是如果再次执行git merge feature/001，你会发现，git不让你merge了，因为之前已经merge过了，很尴尬 

这个时候，过了几天了，然后呢，已经准备好可以上线了，你需要重新将release分支的代码合并到master分支上来，此时该怎么办呢？ 

而且此时一般是说feature分支需要继续修改代码，比如又加入了一些代码，但是如果这个时候你胡乱再次合并，会发现master分支仅仅merge进去了feature分支后来修改的那一点代码而已。。。。 

此时，要做的事情是，继续对feature分支进行修改，完成最终版本的代码 

然后，再次执行git revert HEAD，将master分支再次进行revert，回退到之前merge过后的那个状态，这个状态是包含了feature分支merge进来的部分代码的，然后再次执行git merge feature/001，将feature分支最新的修改也merge进来 

ok了，完美解决，再次git push origin master，推送到远程分支去 

4、结合gitlab实战一下分支合并的撤回操作 

梳理一下 

（1）如果是要撤回已经push到远程仓库的merge操作 

（2）在本地执行git revert -m 1 HEAD，再执行git push origin master，此时本地和远程的提交历史都会多一个commit出来，该commit的内容和合并之前的master指向的那个commit是一样的，同时master此时指向最新的那个commit 

（3）但是后面，如果要再次将release分支和master分支进行合并，此时是需要特殊处理的，再次在本地执行：git revert HEAD，git push origin master。就是再次将master还原到指向一个新的commit，该commit的内容与上一次merge后的那个commit一样，包含merge的内容 

（4）最后再次将release分支与master分支进行合并，此时可以保证release分支所有的内容，都是合并到master分支的 

5、接着上一讲，如果在本地做了commit，同时还推送到了远程仓库，此时要撤回，怎么办？ 

简单了，比撤回merge简单多了，直接git revert HEAD，不就回到了上一个commit的状态，然后再次git push origin master，完美同步撤回远程仓库的commit操作了

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\撤回分支合并的原理.png)

###  26、Git高阶实战技巧：二分查找哪一次提交引入了线上bug

这是一个非常实用的技巧1627098499 

我可以给大家介绍一下，我们在项目中是如何去使用的 

经常可能会出现说，线上有bug，然后排查bug的时候，最终会定位到是哪个类的哪几行代码造成的这个bug，此时就需要去看到底是哪一次提交，谁，对这几行代码的修改造成了这次线上bug，需要去定位责任人，发生时间 

如果是比较严重的case，不是bug，case是什么？case你可以理解为是，bug引发的血案，比如说导致用户无法支付，给公司造成了500万的经济损失，肯定是要追查谁是责任人的 

就是说，如果你们线上发现了bug，然后定位到bug是某个代码文件的某几行代码导致的，此时需要看一下是谁在哪一次commit引入了这几行导致bug的代码 

谁在哪一次commit具体的引入了这个bug，用git提供的二分查找 

git bisect start：开始二分查找

git bisect bad：标注当前这个commit是有bug的

git biset good v1.0.0：用tag来标注说从上一次哪个上线为止，是没有bug的，就是那个tag对应的commit之前都是好的 

此时git会做一个二分查找，比如说两次commit中间差了12个commit，此时会给你定位到第6个commit 

然后你可以对这个版本的代码，运行个测试用例，看看bug能否复现出来 

如果发现这个版本的代码没有bug，那么说明是之后引入的bug，输入git bisect good，会继续往后面二分查找；如果这个版本就有bug，那么说明是之前引入的bug，输入git bisect bad，会继续往前二分查找 

以此类推，直到最后找到最后一个commit，认为就是这个commit开始引入的bug 

git会自动推断出来是哪个commit是实际引入bug的commit，最后会给你打印出来 

最后你需要执行git bisect reset，就是从二分查找状态中恢复到之前的状态

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\二分查找找bug的原理.png) 

### 27、Git高阶实战技巧：让多个项目共享一个子项目的代码修改权利 

如果按照我自己的意思，我都不想讲，涉及到的知识点，是submodule和subtree，这两个东西，很多人，搞技术的，从技术出发，git里真正高深的东西是rebase和submodule 

实际上，我是很不认可这个东西，你要从实际工作场景触发，从实战出发，从需求出发 

实际上我在这个Git高阶实战技巧这一大块里面讲解的内容，90%以上都是我工作中真正在企业场景中遇到过的，真正实用，有用的东西，结合工作场景给大家去讲解 

简单来说，就是如果两个项目在两个仓库中，要共享一个子项目的代码，两个项目的人都可以去修改他，该怎么办呢？ 

但是实际上，在实际工作场景中，我坚决不建议出现这样的情况，大家就是如果有某一个子项目的话，单独交给某个人，然后单独拉一个仓库就可以了 

1、Submodules 

一般来说，submodule使用的场景，就是要在一个父项目下，再建立一个子项目。 

（1）我们先建立一个DbConnector子项目 

git clone git@192.168.31.80:OA/DbConnector.git

cd DbConnector

touch README.md

git add README.md

git commit -m "add README"

git push -u origin master 

（2）将所有工程师的父项目中都注册DbConnector这个子项目

进入某一个父项目，然后在其下创建一个submodule： 

git submodule add git@192.168.31.80:OA/DbConnector.git 

这个时候，会出现一个新的目录，就是DbConnector。而且此时用git status命令，会发现说有.gitmodules和DbConnector两个东西等待提交。 

.gitmodules里面保存了你的子模块和其对应的git url之间的映射关系 

然后执行下面的命令，将子模块提交： 

git add --all .

git commit -m ‘added DbConnector module’

git push origin master 

接着别的研发人员，可以将这个包含了一个子模块的项目克隆到自己本地，但是此时默认是只有一个子模块的目录，里面是空的，没有文件的。 

git clone git@192.168.31.80:OA/oa-parent.git 

或者是 

git pull 

此时需要运行两个命令： 

git submodule init

git submodule update 

才能初始化子模块的目录，同时拉取子模块的文件 

或者也可以简化一点，通过给下面的命令，一次性把上述工作都干了： 

git clone --recursive git@192.168.31.80:OA/oa-parent.git 

（3）对子项目进行代码修改 

1）无论谁都可以跟平时一样，在DbConnector里面，当做是一个完全独立的项目，在里面可以修改源代码，包括说修改之后提交，分支操作，在DbConnector里面去执行就可以了 

2）以后每次如果某个人要更新子模块中的数据，那么执行以下命令即可： 

进入子模块的目录

git fetch

git merge origin/master 

2、Subtree 

git以前是推荐使用submodule来管理子项目，但是git 1.5.2之后就推荐使用subtree了。subtree的优点，就是子项目的维护对我们来说几乎就是透明的，不需要引入太多的维护成本。 

cd P1项目的路径

git subtree add --prefix=用来放S项目的相对路径 S项目git地址 xxx分支 

上面的代码，就会把s项目的代码（指定分支）下载到本地放入指定的目录中，同时提交到本地p1项目中 

对于P2项目也做一样的事情 

直接在p1中各种修改代码和提交，包括s项目的代码，都没有关系 

接着某一天，p1项目负责人想把对s项目的代码更改提交到远程仓库，此时执行下面的命令： 

git subtree push --prefix=S项目的路径 S项目git地址 xxx分支

这个时候，git会找到所有对s目录的commit，然后一起提交到其对应的远程版本库中 

接着p2项目的负责人知道了这个事情，需要将s项目最新的代码更新到自己本地，此时就执行下面的命令即可： 

git subtree pull --prefix=S项目的路径 S项目git地址 xxx分支 

3、最后的一个总结 

如果有多个仓库，多个项目，要共享对同一个子项目的修改权限的话，那么可以这么玩儿 

子项目其实是有一个单独的仓库的 

但是在父项目中可以为其注册子项目，接着不同仓库的人，就可以很方便的在自己的仓库代码中，修改子项目的代码，同时互相之间都可以push代码，也可以pull到别人push的代码 

就算真的有类似的场景，我也不推荐使用，太复杂了

### 28、Git高阶实战技巧：团队拆分迁移项目代码时简化提交历史

这同样是一个非常有用的技巧 

我讲解的这些命令，其实都是git官方推荐的核心和重要的命令以及操作 

就是说，我的高阶的实战部分，涵盖的这些命令，本来就是大家都需要掌握的，而且都是很重要的 

但是我讲课的风格，就是实战驱动，业务驱动，项目驱动 

所以我必须是以我在好多个大公司里面工作过，经历过的各种真实的场景，给大家来铺垫每个git命令的背景，这样的话，大家第一，听的时候不会很枯燥，git rebase，git bisect，背景铺垫，实战场景铺垫；大家结合我说的这些真实的工作场景中，如何运用这些git命令，也可以让大家更好的去理解，你学习的这些东西，可能在未来什么时候会派上用场 

比如我们现在在一个业务团队里面工作，开发了一个工作流中间件系统，支撑了团队里多个项目的发展，挺好，对吧 

然后呢，嗯。。。BAT里某一家最常说的，拥抱变化，拥抱变化 

这个业务团队副总裁觉得干的不咋地，于是决定玩儿一把权术厚黑，把团队里部分项目拆分出去，提拔原来团队里的一个经理变成了总监，带另外一个独立团队，此时会有什么效果，被拆出去的那个新任总监，肯定是会觉得，哇塞，天上掉馅饼，我一下子就成总监了 

肯定是吭哧吭哧吭哧，拼命干，每天干到凌晨3点，每天看着凌晨3点的北京的风景，甚至是偶尔看看凌晨6点北京日出，他也会干 

此时就要。。。4个业务系统可以拆分出去，2个业务系统 

但是，4个业务系统都是由一个支撑性的工作流中间件去支撑的，这个时候怎么办呢？基础支撑性系统呢？？？ 

尴尬了。。。但是团队A的基础系统支撑团队B的业务系统？？？ 

当然不行了，拆！！！ 

基础支撑系统直接拷贝一份代码，上传到另外一个git仓库，给那个团队去维护 

此时团队A就要负责项目代码的迁移，那么就有一个注意的点，一般这个时候，其实都是要抹掉之前所有的提交历史的，让新团队接手的那份代码，commit提交历史就一个初始提交记录 

这个是为什么？因为新团队里会招聘新的人过来，此时如果新的人看以前的commit提交历史，都是别的团队的人的，其实不太好的，跨团队之前，尽量不要有这么多的耦合，否则会弄的大家很迷糊 

一般大公司里面，跨团队，项目代码也是比较敏感的，即使可以拷贝代码，但是说之前的一些提交历史最好还是要抹掉的，看起来这就像一个全新的项目一样 

将原来的一个仓库拆分成两个仓库 

echo 'get history from blah blah blah' | git commit-tree 9c68fdc^{tree} 

我们从要截断的那个commit的上一个commit，创建一个基准commit出来，比如用倒数第二个commit 

git rebase --onto 622e88 9c68fdc 

接着，执行git rebase --onto命令，将倒数第二个commit之后的commit重新基于那个commit基础之上来构建提交历史 

此时，master分支代码就会只保留了2个commit 

然后可以将这个master分支的代码和commit历史提交到另外一个新的代码仓库上去，那个代码仓库就会仅仅保留两个commit了 

我是特意没有将详细的步骤记录下来的 

因为我是希望大家能够自己一边看视频，一边记录下来git的操作记录，然后自己动手去实验，去体会里面的所有的操作 

因为git这块本身不涉及到什么代码，所以还是希望大家自己多动手 

但是后面的内容，正式进去项目开发以后，为了保证每个同学学习的进度和质量，都是会给出非常完整的课程之后自己写代码的实验手册

### 29、Git高阶实战技巧：将新版本的功能放到上一个版本提前上线

现在假设我们同时在开发两个版本，v1.0和v1.1，然后pm突然决定，将v1.1的部分功能提前放到v1.0版本先上线？此时该怎么办呢？

比如说我们同时拉了两个版本在开发，每个版本肯定都是有不同的feature分支的 

假如说此时已经进入了v1.0和v1.1两个版本的release分支测试的阶段 

但是突然发现，老板boss很着急，希望v1.1版本的部分核心功能，提前放到v1.0先上线，v1.1的版本剩余的功能可以慢慢上线 

v1.1版本里面，提交历史，这个时候就来了，为什么之前告诉大家说，一定要按照规范，每天开发好的那一块儿完整的功能，就作为一个commit，后续基于这条清晰的脉络可以做很多事情 

比如说现在就可以从V1.1的提交历史中，找到v1.0需要提前上线的那些功能对应的commit 

如果你没有清晰而且定义良好的commit历史的话，试问，你怎么从一团乱麻的v1.1版本的代码中，找出来指定的那几个功能对应的代码呢？？ 

提前完成测试，直接上线 

很简单，用git cherry-pick功能 

如果我们要做到部分功能提前挪到v1.0先上线，那么就需要将v1.1中那些功能对应的git commit放到v1.0分支上去，然后将那些commit对应的代码都应用到v1.0分支上去，就可以了 

git checkout feature/v1.0

git cherry-pick feature/v1.1分支对应的多个commit标识

### 30、Git高阶实战技巧：基于GitLab的代码权限控制以及强制代码审查

Git高阶实战技巧这一部分的最后一讲，在实际生产环境中，各种高阶的实战技巧 

代码权限这回事儿 

实际上，如果没有gitlab，你会发现你做代码权限的控制很不方便 

但是，有了gitlab之后，你会发现很方便 

在实际企业中，结合gitlab，代码权限控制 

（1）第一件事情，有些分支是需要被保护起来的，就是说不能直接被push，尤其是master分支。 

在settings->repository，有一个protected branch，就是master分支默认是被保护起来的，只有这个项目的master角色的开发人员，才能直接对master分支进行push操作 

绝对不允许任何普通的研发人员随意就可以往master分支上push代码 

也绝对不允许普通的研发人员，可以有权利把merge request合并到master分支上去 

只有拥有master权限的用户才可以s 

我们实践中是怎么弄的，一个仓库对应着一个项目，一个项目总有一个leader，项目负责人，高工，架构师 

总之，一遍就是这个项目里的高工和架构师才有master的权限，可以push代码到master，还有将merge request合并到master去 

（2）仓库全都是private私有的，如果在一个部门中建立了一个项目，那么默认这个部门下的研发人员对这个项目都是有权限的，但是有的人是master，有的人是developer 

developer是只有权限对普通的分支进行各种操作的，feature，develop，release 

但是最后developer是没有权限直接push代码到master分支的 

而且，developer最后是需要提交merge request，将release分支合并到master分支的，此时只有master权限的人才可以去review merge request，将其他分支merge到master分支 

其他团队的人，其他group的人，默认是没有权限来访问我们部门的项目的 

每个部门的人，默认就只能访问自己部门的项目和代码 

每个项目的话呢，又只有master负责人有权限操作master分支 

其他团队的人可以作为guest来访问我们团队的项目，因为经常会遇到什么呢？有其他团队的人说，让我看一下你们的代码行不行啊，那此时可以啊，给一个reporter权限就可以了 

guest，你可以给一些大领导，有些大领导是不需要修改代码的，可能也不看代码，然后就是来看看你的项目里的一些issue，gitlab管理一些缺陷，大领导，可能会关注一些bug的情况，会进来看看bug相关的一些issue 

（3）实践过的一个技巧：强制code review 

强制code review，每个工程师在feature分支中开发好和单元测试好之后，他们本来是需要将feature合并到develop去做集成的 

其实这个环节是需要进行code review，要求他们必须提交merge request到develop分支，让项目里的master权限的高工去review他们的代码 

但是如果默认情况下，没有将develop分支设置为protected，那么普通人是可以绕过高工，直接在本地跟develop分支进行合并，然后push到远程的develop分支上去的 

如果将develop分支保护起来之后，就是说，每个研发人员第一次写好feature分支的代码，都必须通过merge request往develop分支合并代码，此时项目里的高工会负责review他们的代码 

review通过之后，可以合并到develop分支，做持续集成 

后续是要在develop分支做持续集成测试的，可能需要修改bug，修复集成测试的bug，还是在feature分支上进行。每次修复好一批bug之后，push到feature分支，然后提交merge request合并到develop分支，高工继续review代码。 

就是说卡在持续集成这个环节，将代码质量给严格把关，每次提交的代码，都必须高工过目，看一眼，不能写的太烂 

通过develop持续集成之后，会从develop拉那个release分支，接下来，每个研发人员就可以在release分支直接进行bug的修复了

### 31、Git内幕原理：深入Git内幕研究的引子介绍

从这一讲往后，我们会不少讲，是进入这个git课程的最后一块内容，就是git内幕原理1627098499 

简单带一句，我现在的一个授课思想，刚开始学习一个技术，必须先短平快引入，入门要快，把这个技术最核心需要用到的一些东西，用一点简单的demo带一些，快速的就入门了 

在入门的同时，用的是那个技术最核心的一些东西，直接把这个技术最最根本，最最基础的原理，给大家阐明 

刚有个感觉，体验，功能背后的一套比较根本的原理是那样子的，三个区域 

就是纯实战驱动，这个技术在实际企业中是怎么来运用的，如果是类似maven，git这种技术，是没法做项目实战的，融入在我们后面的大项目里的，贯穿了整个做项目的环节 

但是我们可以把企业中常见的一个一个的实战场景，应用场景，反过来推出，我现在有这么一个企业里的而应用场景，需求，问题，然后呢，我的git技术有哪个命令或者说是功能，可以解决他？？ 

每个技术讲解完之后，每个git的命令，如果有可能，再给大家带一带，深入剖析一下命令背后的原理。功能分支工作流实战切入，分支那一套的内幕原理，后面的一系列的命令 

实战驱动，深入原理 

收尾的那一段，精华，比如说git，其实它是有自己的一套内幕原理的，非常深入的去探索一下git内幕里的原理，把深入的最后一块东西给讲好，整个这个技术学习，完美 

（1）学习一些开源技术背后的原理和思想 

（2）加深你的技术内功，会用一大堆技术有什么用？但是如果你对很多技术背后的内幕原理和整体上的架构设计思想，都很精通，那么你的技术内功会很强悍 => 你在后面做项目的时候，设计，解决问题，得心应手 => 内功越深厚，你以后在学习类似技术，或者其他技术的时候，速度就会更快 

（3)直接能够提升你的以后做自己项目时候的技术能力 => 吸收了很多开源技术的设计思想，架构，方案，机制 => 运用到了你自己的项目里，你会发现很多问题和场景，都可以吸取他们的思想去解决 

（4）如果你的很多技术内功很深厚的，就业，职场发展 => 99个程序员的简历上写的都是熟悉spring boot，spring cloud等技术的使用，有过什么什么经验 => 深入阅读过spring boot和spring cloud的源码，对其架构体系和设计原理较为精通 => BAT，简历上的亮点 => 面试的时候，如果你能把这些深入的东西讲出来，架构师，技术专家，高级工程师，BAT，中小型公司，都会有很大的优势 

一张思维导图带你梳理清楚学习到的技术体系，maven课程，反馈都很好 

授人以鱼不如授人以渔：学习方法，如何去自己去未来更加深入的研究和学习相关的技术，自己如何能够走得越来越远 

包括我之前讲的很多讲，Git高阶实战技巧，讲了一大通，“废话”，精华 

光讲技术，是没什么稀奇的 

稀奇的是，我在公司里什么场景下，是怎么用这个技术的，经验，会花很多时间去给大家铺垫各种git命令讲解之前的场景，那些东西我认为比git命令更重要，几十个git命令，你都不知道用来干嘛的，过了2个月，保证你全部忘记 

记忆曲线，如果脑海中有个场景，有个图片帮助你记忆，你的记忆的遗忘会慢很多，记忆清晰很多，记忆的时间会长很多 

我觉得，知其然而知其所以然，你不要光学，你为什么要学，你学了以后有什么用

BAT，晋级答辩，职级体系，升级打怪

每次去要网上晋升一级的时候，非常强调你的思考的能力，你不要告诉我你做了什么？你告诉我，你为什么要做，你为什么要这样做，你这样做了以后效果是什么，作用是什么？ 

技术评审委员会的成员之一，参加过很多初中高级工程师的晋级答辩，工程师，what，我做了什么 

技术工程师，必须要懂得思考，梳理，总结，抽象，提取。为什么做，为什么要这样做，这样做以后我对公司的产出是什么，价值是什么，业务的促进是什么 

软素质真正强悍的人，才可以去做技术经理，技术总监，CTO，CEO 

1、Git内幕研究的一个思路 

git add，git commit，git push，这些命令背后在干什么？git内幕原理 

不是纯讲，要在动手实践中，去给大家讲请清楚这些命令背后的原理是什么？ 

porcelain命令，这些命令，指的就是类似git add，日常开发中，程序员会直接使用的git命令 

这些porcelain命令底层都是调用的一些git的底层命令，plumbing命令，实际上是不是给我们用的，是给git的那些高阶命令去用的 

我们在这里讲解git add这些高阶命令背后的原来，是带着大家动手实践去体验一下底层的命令是用来干嘛的，当你把那些对应的底层命令都学会之后，明白那些底层命令在干什么，接着你就会瞬间明白，git add，git commit这些高阶命令，你在执行的时候，他们背后在干什么 => 内幕原理，你就全部清楚了 

2、.git目录研究 

平时我们如果执行git init之后，就会生成一个.git目录，其中就是git存放所有数据的地方。如果我们拷贝这个.git目录，那么就相当于备份了一个完整的git项目的数据。 

在.git目录中，有如下一些内容： 

config

description

HEAD

hooks/

info/

objects/

refs/ 

description文件仅仅是由Git WebUI程序去使用的，我们一般不用去管这个文件 

config文件包含了这个git项目的所有配置项 

info目录中包含了我们在.gitignore文件中定义的不需要git追踪的那些被排除掉的文件 

hooks目录中包含了我们的client-side或者是server-side的钩子脚本 

其实上面那些都不是最重要的，最重要的是这几个东西：HEAD文件，indexes文件，objects目录，以及refs目录。 

objects目录存储了git中所有的数据，refs目录中存储了所有的指针（包括了branch，tag，remote，等等，指向了objects目录中的数据），HEAD指向了当前我们所处的分支指针，indexes文件存储了暂存区中的内容。 

把接下来的课程听懂之后，你就会了解常见的git命令背后的原理，还有git命令结合.git目录的存储相关的原理

### 32、Git内幕原理：在动手实践中掌握提交代码的内幕原理

（1）git数据存储机制 

1）git hash-object初体验 

git的数据存储格式实际上是一种非常简单的key-value存储格式。就是说，git支持将任何东西存储到其数据库中，一般就是存储文件的内容，然后用一个key值来引用它。 

首先随便找一个目录，初始化为git项目： 

git init 

此时去.git/objects目录中检查一下，发现只有两个子目录，info和pack，没有任何文件，此时数据库是空的。 

用下面命令，可以手动将一个字符串作为value存储到git的数据库（objects，git的数据库，用了比较简单的方式去存储）中去，会看到返回了一个hash值来引用那个字符串value： 

echo 'hello world' | git hash-object -w --stdin 

解释一下上面的命令： 

git hash-object就是git的一个底层命令，一般我们是不会直接用的，一般都是git add，git commit之类的命令在调用这种底层命令。 

git hash-object命令，默认的表现是这样的，接受一个value，然后针对这个value返回一个hash值，默认是不将value写入自己的数据库的 

但是使用了-w之后，git就会将value写入数据库之中，同时用hash值作为key来引用那个value对应的文件。同时--stdin指的是从命令行接收value，也就是echo ‘test content’中的test content。如果不使用--stdin，那么要求是在hash-object之后跟一个文件名，它就会将一个文件作为value存储到数据库中去，然后返回一个hash值。 

git返回的是SHA-1 hash值，是40位的字符串，是根据文件内容计算出来的一个checksum，校验和。 

2）git的文件存储机制（轻量级索引机制） 

你往git里存储一个文件，git一定会根据文件的内容计算一个40位的hash值出来，作为那个文件的名称，也是指针，40位hash值就可以来引用这个文件。存储的时候，40位hash的头2位，会作为目录名称放在objects里面，然后那个存储的文件会放在那个目录中，文件名是40位hash值的后38位。 

大家来思考一下，git作为一个高性能的版本控制系统，一大特点就是性能超高，在切换分支的时候，都需要进行大量的磁盘读写，磁盘存储机制是比较值得我们注意的 

将40位hash值的头2位作为目录，其实你可以认为是一种轻量级的索引机制 

这样的话，有一个好处，就是每次你要根据一个hash值定位一个数据的时候，直接根据头2位先定位到一个目录，然后再在这个目录下去查找，linus写git的时候，就是用了一种非常轻量级的文件索引机制 

直接查看git存储的文件的内容是不ok的，我们后面会讲解要用git专用的底层命令来查看 

3）git cat-file体验 

使用下面的命令可以查看刚存储进去的文件值，git cat-file是另外一个底层命令，就是专门用来查看存储到git中的文件的内容的： 

git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4 

4）一个文件的多个版本存储机制的体验 

下面的命令可以将一个文件存储到git数据库中： 

echo 'version 1' > test.txt

git hash-object -w test.txt 

echo 'version 2' > test.txt

git hash-object -w test.txt 

git会将一个文件的多个不同版本，以完整快照的方式存储在objects数据库中，每个版本都用一个单独的文件来存储，每个版本的文件都有一个不同的hash值。 

使用下面的命令可以查看.git/objects中所有的文件：find .git/objects -type f 

接着可以试着将这个文件给删除，然后从git数据库中再恢复回来这个文件： 

rm -rf test.txt

git cat-file -p 83baae61804e65cc73a7201a7252750c76066a30 > test.txt

cat test.txt 

也就是说，我们只要将数据存储到了git中，那么就不用担心丢失了，随时都可以找回来这个文件的任意一个版本 

我们用下面的命令可以查看一下git中存储的文件的类型，默认都是blob： 

git cat-file -t 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a 

这里，我们就发现跟之前给大家讲解的那个原理就可以串起来了，因为实际的文件的内容，每个文件的版本快照，会作为一个blob object存储在git数据库中 

git数据库，git仓库，database，repository 

5）梳理一下 

实际上，就是一个文件的不同的版本，都会用一个完整的快照的形式，作为一个单独的文件存储在git数据库中，所谓的完整快照，每个版本，都是用一个文件存储这个版本的完整的内容的 

每个版本的文件内容，都是通过blob object的类型存储在git数据库中的 

（2）tree object 

git有另外一种object叫做tree object，每次将一堆文件的一个版本存储到git仓库中，都是用blob object来存储那些文件的版本，然后用一个tree object来引用多个blob object。tree object通常会包含指向其他多个blob的指针，或者是其他tree object的指针。用下面的命令，可以查看master分支指针指向的那个tree object： 

git cat-file -p master^{tree} 

git通常是基于暂存区中的内容来创建一个tree object的，我们首先需要在暂存区中加入一些内容，然后可以试着手动创建一个tree object。可以将仓库中存储的某个commit object的内容放入暂存区中： 

git update-index --add --cacheinfo 100644 83baae61804e65cc73a7201a7252750c76066a30 test.txt 

git update-index：底层命令，一般是git add命令去调用的，将数据库中存储的某个blob数据的引用放到暂存区中 

--add：因为暂存区中还没有这个文件，你可以认为这个文件是第一次进入暂存区，所以需要这个选项 

--cacheinfo：不是从工作区中加入文件放到暂存区，是从git仓库（git数据库）中加入文件 

100644 mode：这种模式的意思是，这个是一个普通的文件 

接着使用下面的命令基于暂存区中的内容，创建一个tree object： 

git write-tree：将暂存区中的内容创建为一个tree object，tree object也是放入了git仓库，主要是包含了对一个blob obejct的引用 

git cat-file -p d8329fc1cc938780ffdd9f94e0d364e0ea74f579

git cat-file -t d8329fc1cc938780ffdd9f94e0d364e0ea74f579 

下面我们再加入一个new.txt文件放入暂存区中，同时将test.txt文件的另外一个版本放入暂存区中，再次创建一个tree object： 

echo 'new file' > new.txt

git update-index --add new.txt，这个就是将工作区里的内容直接放入暂存区

git update-index --add --cacheinfo 100644 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a test.txt 

git write-tree

git cat-file -p 0155eb4229851634a0f03eb265b69f5a2d56f341 

此时会发现这个tree object对应着两个文件 

下面的命令可以从仓库中读取一个tree object放入暂存区，然后再基于这个tree object创建一个新的tree object出来： 

所谓的将仓库里的东西读出来放到暂存区，实际上只是将一些引用放到暂存区，但是blob，tree，实际还是存储在objects中的 

git read-tree --prefix=bak d8329fc1cc938780ffdd9f94e0d364e0ea74f579

git write-tree

git cat-file -p 3c4e9cd789d88d8d89c1073707c3585e41b0e614 

此时tree object包含两个文件和一个tree object，如果将这个tree object的数据恢复到工作区中，就会有两个文件和一个叫做bak的文件夹，bak文件夹中也包含了一个文件。 

（3）commit object 

随着你不断的在工作区中编写代码，然后呢，每次如果执行了一些相关的操作，可以将当前这个时刻，有变化的文件的版本都存储到git仓库中，同时放入暂存区中。 

此时，暂存区中，没有变化的文件，还是保持着之前的版本；有变化的文件，暂存区中会存放最新版本的blob对应的引用 

然后，如果此时创建一个tree object，就是基于暂存区中当前所有的文件的版本的blob，创建一个tree 

此时，这个tree object就代表了这个项目的所有代码的此时此刻的一个完整的快照 

现在我们实际上是有3个tree object，分别代表了项目的不同时刻的快照。但是此时此刻，我们有个文件，就是不知道那些tree object是谁添加的，为什么添加，什么时候添加的，什么都不知道。 

但是，光有tree object是不够的 

此时就需要commit object了。 

用下面的命令，基于已有的tree object创建一个commit object，同时给这个commit object一个提交备注： 

echo 'first commit' | git commit-tree d8329f 

然后查看一下这个commit object，会发现包含了一个tree object，同时又作者，提交日期，以及提交备注： 

git cat-file -p fdf4fc3 

commit object的含义很简单，就是指向了一个tree object，而那个tree object指向了多个blob，其实这个tree object就代表了某一时刻项目中所有代码文件的快照版本，同时那个commit object包含了作者、提交时间、提交备注，此外还包含了指向上一次commit object的指针。 

同样的，我们可以为另外两颗tree object分别创建一个commit object，代表了另外两次提交： 

echo 'second commit' | git commit-tree 0155eb -p fdf4fc3

echo 'third commit' | git commit-tree 3c4e9c -p cac0cab 

此时相当于我们就有了三次跟真实一样的提交了，可以用git log来查看一下提交历史： 

git log --stat 

就会看到最近的3次提交 

其实上面的实验步骤就彻底揭示了git的底层原理了git add和git commit命令执行时使用的就是上面那些低级别的命令： 

git add：相当于是将你的有修改的文件作为一个新的版本，采用单独的blob文件存储到仓库中去，同时将仓库中的文件更新到暂存区中 

git commit：相当于就是将暂存区中的文件快照，也就是对应的blob指针创建一个tree object，写入仓库中，同时再创建一个commit object包含了提交人/提交时间/提交备注，来指向那个tree object，代表了一次提交对应的仓库快照 

blob、tree和commit，三种object都是作为一个独立的文件在objects目录中存储的，存储时的子目录命名和文件名，都是基于hash值来的 

最后用一张图来说明，到目前为止，整个commit object、tree object和blob的关系。 

（4）SHA-1 hash值如何计算 

首先blob的内容分为两块，一个是header，一个是content。header就是类型+空格+长度+\u0000。然后会用header+content来计算出一个SHA-1校验和，作为hash值。 

（5）梳理一下 

git update-index --add 文件名：其实就是，我们在工作区里修改了文件以后，会用这个底层命令，将工作区中有修改的文件的一个版本，放入git仓库中作为一个blob去存储这个版本，同时将这个blob的引用放入暂存区中 

多次有git update-index之后，git仓库中会存储项目中每个文件的所有版本，同时在暂存区中保留了每个文件的最新版本，没有变化的就保留值钱的版本 

执行git write-tree，就可以针对暂存区中当前项目的最新版本，创建一个tree object，引用项目中所有的文件当前此时此刻的一个版本对应的blob，tree object，就代表了项目当前的一个完整快照版本 

接着执行git commit-tree，就可以基于这个tree object创建一个commit object，包含了tree object的创建人，创建时间，备注信息，等等，作为一次提交 

（6）将上层命令和底层命令结合起来 

1）我们首先在工作里各种新增、修改文件 

2）我们执行git add --all .这个命令：此时会将工作区中有变化的文件，作为一个新的版本，直接存储到git仓库中去，作为一个blob来存储；同时在暂存区中加入这些最新版本的blob的引用，替换同一个文件之前在暂存区中的版本 

git update-index 

3）我们执行git commit，同时给出一个提交备注：此时会针对暂存区中现在的所有的版本，创建一个tree object，作为项目此时此刻的一个快照版本，放入仓库；另外再创建一个commit object，包含了提交人，提交时间，提交备注，来引用这个tree object 

4）到此为止，完成一次git add和git commit 

5）整个git内幕原理，剖析的非常清楚了 

（7）最后再来一张图，结合命令，演示一下 

1）编写两个文件，test1.txt和test2.txt 

2）git add --all . 

将两个文件的第一个版本，作为两个blob保存到了Git仓库中，同时也放入了暂存区中 

3）git commit -m "第一次提交" 

基于暂存区里的两个文件的第一个版本，创建了一个tree 

然后基于提交信息创建了一个commit，引用了那个tree，此时这个commit就代表当前这个项目此时此刻的一个完整的版本 

4）修改test1.txt位第二个版本 

5）git add --all . 

将test1.txt的第二个版本作为blob存储到git仓库中 

同时将test1.txt的第二个版本覆盖了暂存区中的test1.txt的第一个版本 

6）git commit -m "第二次提交" 

创建了一个tree，引用了test1.txt的第二个版本，test2.txt的第一个版本 

另外创建了一个commit，引用了那个tree，代表了此时此刻项目的一个完整的版本 

7）每个commit，就是当前项目的一个完整的快照版本，包含了项目的所有文件的一个最新版本 

8）为什么大公司里，规范项目里，对git commit都有要求的，不要随便瞎提交，都是完成一个完整的功能或者模块之后再提交一次，尤其是每天提交一次，或者每天提交多次，每次代表了一个完整的功能和模块 

9）整个你的项目的提交历史，就是项目版本的变迁的一个完整的历史，每个commit都代表引入了一个新的功能或者是模块

git object底层命令原理图

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\3201.png)  

 git提交的内幕原理

 ![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\3202.png)

###  33、Git内幕原理：在动手实践中掌握Git指针的内幕原理

.git/objects里面的内容是怎么回事儿，跟git hash-object/git update-index/git write-tree/git commit-tree等底层命令的关系是怎么回事儿，底层命令跟git add和git commit命令结合起来是怎么回事儿，图解1627098499 

git提交那一块，你完全掌握了底层的内幕原理了 

除了存储那些数据，git里面有很多种指针，tree/commit里面本来就含有指针，指向其他的blob或者是tree，暂存区里面也是指针 

分支、HEAD、tag、remote 

.git/refs这个目录里面的内容 

（1）分支指针 

如果我们要查看某个commit，那么还是要知道它的hash值，然后用git log hash_value来查看这个commit。但是这还是很麻烦，所以最好是可以有个文件存储那个hash值，然后我们直接用一个较为简单的名称来引用。 

.git/refs目录中，有几个子目录，包括了heads和tags两个子目录，在这里就有各种指向objects中的指针。 

.git/refs/heads目录下，有一个master文件，实际上是git自动创建一个初始的分支，分支其实就是一个指针而已，指向了某个commit object的，在master文件中就包含了一个40位的hash值，当前就是指向了最近一次commit object 

.git/refs/heads目录下，会包含很多个文件，就是每次创建一个分支，就会在这个里面对应新建一个文件，文件名称就是分支的名称，文件里的内容就是分支指针当前指向的那个commit object的hash值，代表着这个分支当前指向了哪个commit object 

我们下面动手实践一下，用git底层的命令，看看，我们手动去创建分支会是什么样的效果 

我们可以用下面的命令创建一个名叫feature/001的指针，指向了第一次提交的commit object 

echo 1a410efbd13591db07496601ebc7a059dd55cfe9 > .git/refs/heads/feature/001 

接着用下面的命令，就可以查看master指向的commit object之前的commit历史 

git log --pretty=oneline feature/001 

总结一下，git里面的分支的内幕原理，大家就彻底清除了，新建一个分支是很轻量级的一个事情，不是像其他的版本控制系统那样，新建一个分支，是将所有的文件内容和版本都拷贝一份 

其实在git中，提交历史，blob+tree+commit，是最核心的，所有的数据都是通过这个方式来存储一份 

然后呢？新建一个分支，实际上就是在.git/refs/heads下面新建对应的目录和文件，在文件里面维护一个指针，一个hash值，指向了.git/objects中某个commit object的hash值 

通常不建议直接自己手动修改refs文件的内容，可以用下面的命令更新某个refs文件指针指向的commit object： 

git update-ref refs/heads/feature/001 1a410efbd13591db07496601ebc7a059dd55cfe9 

其实上面就已经揭示出来了，这就是git中的分支机制的根本原理，分支就是指针而已，分支指针指向了不同的commit object。用下面的命令可以再创建一个test分支，让其指向某个commit object： 

git update-ref refs/heads/test cac0ca 

实际上我们如果执行git branch命令，就是执行update-ref命令，创建那个分支对应的文件，然后文件内容就是对应的commit object的SHA-1 hash值。 

删除一个分支很简单的，就是删除.git/refs/heads下面的分支对应的目录和文件即可 

（2）HEAD指针 

HEAD文件中，保存了对某个分支指针的引用，其实就是某个分支对应的文件。 

HEAD其实也是一个文件，但是也是一个指针，这个HEAD呢，就是指向了当前你所处的那个分支而已，保存的是refs/heads下面的分支对应的目录和文件名 

比如当前你在master分支，那么HEAD文件中的内容就是refs/heads/master，代表了HEAD当前指向了master分支 

再比如当前你在feature/001分支，那么在HEAD文件中，内容就是refs/heads/feature/001，代表了HEAD当前指向了feature/001分支 

因此如果我们比如在master分支上，创建并且切换到了另外一个分支，那么此时会根据HEAD找到当前分支文件，再找到当前分支指向的commit object，那么新建的分支就会同样指向那个commit object。 

从比如master分支切换到feature/001分支，实际上就是一个非常轻量级的操作

 （1）HEAD文件里的内容，从refs/heads/master，变为refs/heads/feature/001，代表着HEAD指针指向了feature/001分支 

（2）将feature/001分支指向的那个commit对应的tree，对应的那些blob对应的文件版本的内容，一次性从仓库里恢复到工作区中，工作区中所有文件的版本和内容，会变成那个分支指向的commit当时的那个快照版本 

每次切换分支之后，HEAD都会指向那个分支的文件 

可以通过git checkout命令+cat .git/HEAD命令结合起来，然后就可以看到每次切换后的HEAD值 

每次我们执行一次git commit操作，都会新建一个commit object出来，然后会让当前分支指向最新的commit object。 

通过下面的命令可以读取和修改HEAD文件的指针： 

git symbolic-ref HEAD

git symbolic-ref HEAD refs/heads/test 

到这里为止，.git/refs下面最重要的东西就讲解完了，分支和HEAD 

分支就是一个文件，保存了指向的commit的40位hash置 

HEAD也是一个文件，保存了指向的那个分支对应的文件名称 

（3）tag object 

其实除了blob、tree和commit三种object之外，还有一种object就是tag object。tag object是指向某个commit object的，包含了标签时间，标签名称，等等。而且tag object永远就指向创建时指定的那个commit object，不能修改。 

我们可以通过下面的命令创建一个轻量级的tag object 

就只是一个指针而已，在refs/tags下面，会有一个tag名称对应的文件，然后里面就是那个tag指向的commit的40位hash值 

git tag v1.0：轻量级的tag，直接就指向了当前分支指向的那个commit object，就是在refs/tags下面搞一个问加你，作为一个指针而已 

git update-ref refs/tags/v1.0 cac0cab538b970a37ea1e769cbbde608743bc96d 

当然也可以创建一个带备注的标签：git tag -a v1.1 -m 'test tag' 

此时查看cat .git/refs/tags/v1.1，会发现返回的不是我们指定的那个commit object，是另外一个新创建的tag object的SHA-1 hash 

git cat-file -p 9585191f37f7b0fb9444f35a9bf50de191beadc2 

再次查看那个tag object的hash值，此时可以显示出来其指向的那个commit object，同时包含了tag的创建时间、创建人、备注，等等信息。 

（4）remote 

最后一种引用类型，就是remote。 

如果我们添加了一个remote，同时push了一些东西到那个remote，git会在refs/remotes中保留对每个分支，最近一次push到远程服务器对应的commit。 

比如下面的命令： 

git remote add origin git@github.com:schacon/simplegit-progit.git

git push origin master 

cat .git/refs/remotes/origin/master 

查看一下上面的文件，会看到一个SHA-1 hash值，就是这个分支最近一次push到服务器的object commit的SHA-1 hash值。 

FETCH_HEAD，就是最近一次fetch下来的每个分支对应的commit hash值，就在这个里面 

（5）梳理一下 

分支：.git/refs/heads

HEAD：.git/HEAD

tag：.git/refs/tags

remote：.git/refs/remotes

FETCH_EHAD：.git/FETCH_HEAD

### 34、Git内幕原理：在动手实践中掌握Git文件合并的内幕原理

我们来通过一个大文件的存储以及git对这个文件多版本存储的优化，作为例子，来看一看git对大量的文件在存储上是如何做优化的 

用下面的命令将一个大文件存入git中： 

curl https://raw.githubusercontent.com/mojombo/grit/master/lib/grit/repo.rb > repo.rb

git checkout master

git add repo.rb

git commit -m 'added repo.rb' 

然后看一下master指向的commit object对应的tree object包含的内容，master分支指向了一个commit object，commit object是指向了一个tree object，tree object指向了此时此刻项目中所有文件的一个最新版本 

git cat-file -p master^{tree} 

用下面的命令看一下新加入进去的文件有多大： 

git cat-file -s 033b4468fa6b2a9547a70d88d1bbe8bf3f9ed0d5 

在文件中加入一些新的内容，然后再次提交： 

echo '# testing' >> repo.rb 

git commit -am 'modified repo a bit'：直接将工作区中的修改过的文件加入暂存区，再执行一次提交，但是我一般不常用 

再次查看master对应的最新一次commit object对应的tree包含的blob，发现原来的文件换了一个，也就是修改后的文件重新存储了一份 

git cat-file -p master^{tree} 

总结：哪怕是很大的文件，比如说20kb的一个代码文件，每次改动一点代码，git也是存储改动后的版本对应的一个完整的文件；如果这个20kb的代码文件修改了100次，会怎么样？存储100个版本对应的文件，也就是100个文件，2000kb ~ 2MB。所以这样的话对存储的压力还是蛮大的。。。。 

git实际上是可以更加智能的处理这种文件改动的。默认情况下，git会使用loose格式存储文件（loose，松散格式，采取这个文件的每个版本，就存储一个完整的单独的文件），但是一段时间过后，git会将多个loose格式的松散文件打包到一个packfile二进制文件中，来节省磁盘空间。 

什么时候git会进行packfile打包的操作呢？两个时机：我们手动执行git gc操作，或者是将文件push到远程服务器的时候，git会看一下loose格式文件是不是太多了，如果是，则进行打包合并。 

git gc：garbage collector，jvm gc是不一样的，git用来处理自己的文件的 

如果我们将本地的commit数据push到了远程服务器后，就代表远程服务器包含了我们的数据了，此时git就可能会执行一次packfile打包 

此时我们可以手动执行一下git gc命令，来看看效果 

此时再次用下面的命令看一下objects目录下的内容：find .git/objects -type f，之前应该是一堆hash值组成的目录和文件，包含完了十几个object（blob，tree，commit），会发现出现了objects/info/packs和objects/pack等新的目录。 

我们是将之前所有的hash值组成的文件打包成了一个packfile文件，进行了压缩以及合并 

pack包含了两个文件，一个是packfile包含了打包的所有数据，一个是index文件，包含了每个blob object在packfile中的offset，通过.idx文件标识出每个object在.pack文件里是哪一个部分 

git做文件存储的优化，两块：多个文件打包成给一个，压缩，二进制格式，packfile，节省空间；在打packfile的时候，就会对我们刚才说的那种情况，大ruby文件，仅仅保留第一个版本对应的完整内容，然后之后的版本都是直接保留的是它的那些delta增量内容，后面的版本就不是全部保留全量内容了，进一步节约空间 

git，git gc；git push，自动会做打packfile 

实际上，在git进行打包packfile的时候，就会对一个文件的多个版本，仅仅保留其每次增量修改数据，避免对一个文件保留多个全量的版本快照。 

用下面的命令看一下： 

git verify-pack -v .git/objects/pack/pack-978e03944f5c581011e6998cd0e9e30000905586.idx 

会发现ruby大文件，第一个版本仅仅包含9kb的内容，但是第二个版本包含了22kb的内容，所以这里就做了增量处理和优化，仅仅对一个大文件的最新的版本保留了全量的内容，但是对之前的版本都是增量内容 

总结一下 

（1）git gc / git push，会执行packfile的压缩优化

（2）将.git/objects下的各种object对应的文件，压缩成一个packfile

（3）每个packfile都对应一个.pack文件和一个.idx文件，.pack文件包含了多个object的内容，.idx文件包含了每个object在那个.pack里面的offset偏移量

（4）在打packfile的意义：多个文件合并一个，节省空间：对大文件进行增量处理，仅仅是最新的版本保留全量内容，之前的版本保留增量内容

（5）用git verify-pack -v命令，可以查看.pack文件里包含的具体内容

### 35、Git内幕原理：在动手实践中掌握远程分支的内幕原理

对于远程仓库的分支，本地都是有一个对应的分支是追踪那个远程分支的，同时本地还有一个分支是跟本地追踪分支关联起来的 

远程仓库：master 

本地仓库 

1、origin/master，本地追踪分支，跟远程的master完全是需要对应起来的，远程的master指向哪个commit，每次git fetch origin，就会将远程仓库的提交历史拉取到本地跟本地的提交历史进行合并，同时将本地的origin/master指向远程仓库的master指向的那个commit 

本地分支（origin/*），追踪了远程的分支（*），每次git fetch之后，提交历史合并之后，本地分支（origin/*）指向的commit都会跟远程分支保持一致 

不只是origin/master，origin/feature/001，origin/feature/002，origin/release/v1.0 

2、master，跟origin/master关联起来的 

本地的master，是跟本地的origin/master关联起来的，作用是在git pull和git push 

（1）如果执行git pull，会执行远程和本地的提交历史进行合并，origin/master会指向远程仓库master指向的那个commit，同时将origin/master指向的commit跟本地的master指向的commit进行合并，合并之后会出现一个新的commit，同时master会指向那个合并后的commit，然后那个origin/master不变还是指向原来的那个commit 

git pull，相当于是两个命令：git fetch origin + git merge origin/master 

相当于就是将远程仓库的提交历史拉取下来，然后合并在一起，本地的origin/master指向最新的commit，接着将本地的master与origin/master进行合并 

1）将远程仓库的提交历史拉取到本地进行合并，一个commit可能会出来多个分叉

2）同时将origin/master跟远程仓库的master指向一个commit

3）将本地的master和origin/master进行合并，可能需要解决冲突 

（2）如果执行git push，会将本地的origin/master指向master指向的那个指针，同时会将本地提交历史推送到远程进行合并，然后远程仓库的master分支会指向最新的那个commit 

每次执行git remote add命令之后，实际上就会在.git/config文件中加入一段配置，类似下面： 

[remote "origin"]

​    url = git@192.168.31.80:OA/test-project.git

​    fetch = +refs/heads/*:refs/remotes/origin/*

[branch "feature/001"]

​    remote = origin

​    merge = refs/heads/feature/001

[branch "master"]

​    remote = origin

​    merge = refs/heads/master 

我们之前跟大家说过，如果你在本地想搞一个跟类似origin/feature/001关联起来的本地分支，让git push和git pull有效果，git checkout -b feature/001 origin/feature/001，这段命令执行之后，就会在.git/config中加入[branch]配置，标注了本地分支跟origin/*追踪分支之间的关联关系 

那么什么是refspce呢？实际上就是fetch后面的那串东西，+refs/heads/*:refs/remotes/origin/*，就是refspce 

refs/heads/*，代表的是远程仓库被追踪的分支，远程仓库的master

refs/remotes/origin/*，代表的是本地仓库用来追踪远程仓库的分支，本地仓库的origin/master分支

+，代表的是在执行git pull的时候，不是要将远程分支和本地分支进行merge么？即使不是fast-forward的类型，是3-way merge也是要去执行的 

在执行git remote add命令的时候，本地的git客户端会执行fetch命令获取远程仓库的refs/heads/*下面的分支，然后将其写入本地的refs/remotes/origin/*下面去。所以之前说的那些本地追踪远程的分支，比如origin/master之类的，实际上指的就是refs/remotes/origin/master代表的分支 

origin/master等等追踪分支，在本地是放在哪儿的呢？ 

我们是可以手动指定多个refspce的 

根本就不用的下面的语法，介绍一下，refspec指定多个，一般不用 

[remote "origin"]

  url = https://github.com/schacon/simplegit-progit

  fetch = +refs/heads/master:refs/remotes/origin/master

  fetch = +refs/heads/qa/*:refs/remotes/origin/qa/* 

[remote "origin"]

  url = https://github.com/schacon/simplegit-progit

  fetch = +refs/heads/*:refs/remotes/origin/*

  push = refs/heads/master:refs/heads/qa/master

远程分支内幕原理

![](C:\Users\zy199005\Desktop\中华石杉\images\java\02\3501.png) 

###  36、GitHub：学习如何维护开源项目以及给开源项目贡献代码

我觉得我现在是没什么太大必要去讲解这块的

因为这个开源项目的贡献，包括自己开源了一个项目，基于github怎么玩儿，也是包含在git课程里的，而且之前我在录制的时候有一个同学就问了我这个问题 

1、注册一个github账号 

（1）会让你填写一个自己的用户名，我当时因为是给龙果在讲课，roncoo-eshop 

2、发布开源一个项目
 （1）start a project 

（2）将自己本地的一个项目代码上传到github上去 

git remote add origin https://github.com/roncoo-eshop/test-project.git

git push -u origin master 

（3）管理collabrator，这些人是你的开源项目的重要合作伙伴，他们是可以审核别人提交的pull request代码合并请求的 

（4）你自己当然平时可以写代码了 

你当然是可以自己拉个什么分支，在那个什么写代码，但是为了维护开源项目的版本

（5）比如你现在要搞一个v1.0版本，拉一个对应的分支出来 

（6）接着，包括你在内的所有人都依据下面的流程来对v1.0这个版本进行贡献 

3、给开源项目贡献代码 

给开源项目贡献代码，跟公司里的是不一样的，不是说直接就可以对某个分支进行push的。是需要执行一个fork，相当于是在自己的账号里，克隆出一份完整的代码，然后自己就在自己命名空间的fork项目里进行拉分支，开发，最后请求到原始开源项目里进行合并。
 如果要给某个开源项目进行贡献，通常都是在GitHub上，fork一个开源项目，fork之后这个项目就会有一份副本存在于我们自己的命名空间中，这样我们不需要开源项目的push权限也可以贡献代码了 

（1）fork出来一份，在自己的账号里有一个拷贝的项目，同时将自己的ssh key加入github，将fork出来的项目克隆到本地 

（2）从fork出来的项目的master分支创建一个自己的分支出来 

（3）在那个分支上提交代码 

（4）push分支和代码到自己fork出来的项目 

（5）在github上提交一个pull request，对开源项目提交

### 37、思维导图：带你梳理学习到的Git知识体系

课程没有涉及到的东西
	Git可视化工具
		Eclipse/Intellij IDEA + Git
		SourceTree
		Git GUI
		TortoiseGit
		我平时开发
			git命令行
			Eclipse
			GitLab
		讲可视化工具没意义
			如果你掌握我的课程
				大中小型项目开发都没问题了
			如果你要自己去使用某个可视化工具
				精通Git使用
				精通Git内幕原理
				任何一个可视化工具
					官网下载本地安装
					百度搜索基本用法，熟悉一下
					结合掌握的Git原理去摸索使用
			每个人喜好不同
				Git可视化工具在行业里没有垄断型的工具
				随着个人喜好去用
			用起来很麻烦，还不如命令行，降低效率
		程序员多玩玩儿命令行
			有的人认为可视化工具提高生产率
			命令行
				效率很高
				可以让你接触技术本身
				比较酷，看起来像个工程师
			程序员大量的玩儿可视化界面，low

Git入门
	版本控制系统
		什么是版本
		什么是版本控制
		什么是版本控制系统
	版本控制系统的分类
		本地版本控制系统
		集中式版本控制系统：SVN
		分布式版本控制系统：Git
	Git前世今生
	在windows上安装和配置Git
	在本地初始化仓库
	在本地提交代码的不同版本到git仓库
	Git本地的三个区域
		工作区
		暂存区
		git仓库（版本库）
	git add和git commit背后的原理
	git status查看本地项目代码的状态
	git log查看项目的提交历史
	git仓库中的提交历史数据模型
		每次提交就是一个版本
	git reset回退或者快进到任意一个代码版本
	版本的记录+版本的控制

Git分布式多人协作开发
	基于远程仓库的分布式多人协作开发
	基于centos手工搭建一台git服务器
	功能分支工作流
		内部系统+无版本概念
		功能分支工作流的动手实战
		深入图解剖析Git分支背后的内幕原理
			本地分支操作的内幕原理
			结合远程仓库分支操作的内幕原理
		解决分支合并冲突的问题
	基于GitLab搭建服务器
	集中式工作流
		2人以内的极小型项目
		结合GitLab进行集中式工作流的实战
		基于rebase优化集中式工作流的提交历史
	GitFlow工作流
		一个一个大版本稳定进行的项目
		结合GitLab进行了GitFlow工作流的实战
		缺陷：不适合多个版本频繁并行开发的场景
	改进后的互联网公司多版本频繁并行的工作流

Git高阶实战技巧
	实际工作场景中有很多突发问题（大公司，严格规范）
	场景：多个分支之间长期并行时
		查看不同分支之间查了多少个commit
		避免两个分支之间差的代码过大，及时合并
	场景：一次性将多天的代码放入了暂存区
		大公司，敏捷开发，要求一天一个commit，一个功能
		某天忘了提交代码
		有一次将多天代码放入暂存区，需要拆分成多个commit
	场景：feature分支开发到一半让修复bug
		不能将开发一半的代码提交一次再切换到bugfix分支
		将开发一半的代码stash起来，事后再恢复现场
	场景：本地提交历史不规范
		上一次提交备注写的不规范
		前几天的提交备注写的不规范
		将垃圾提交删除掉
		将一天中的多个提交合并为一个
		将多天代码的一个提交拆分为多个
	场景：刚修改的代码或者提交搞错了
		仅仅恢复工作区中的修改
		将工作区和暂存区中的修改都恢复
		将工作区、暂存区和仓库中的修改都恢复
	场景：合并代码后被告知不上线了
		代码合并到master已经push
		revert指令完成合并操作的撤回
	场景：线上bug需要定位哪次提交引入
		git二分查找+测试用例复现bug
		定位到bug责任人
	场景：多个项目都需要对一个子项目修改代码
		submodule
		subtree
	场景：团队拆分迁移项目时抹掉提交历史
		团队拆分后需要拷贝一份代码
		拷贝过去的代码抹掉过往的提交历史
	场景：下一个版本的功能提前到这个版本先上线
		cherry-pick
		将下一个版本的功能commit应用到当前版本
	GitLab权限管理
		团队之间互相不能看项目
		高工/架构师才有master权限
			push代码到master分支
			可以审核别人的merge request
		对develop分支进行保护
			强制将feature分支提交merge request到develop分支
			强制进行code review（代码审查）

Git内幕原理
	高层命令和底层命令
	.git目录下的文件结构
	提交代码的时候深入图解的内幕原理
	深入图解Git指针的内幕原理
	深入剖析Git文件合并优化的设计
		git gc（手动）
		git push（自动）
	深入图解剖析Git远程指针的内幕原理

GitHub
	注册账号
	发布自己的开源项目
		我开源过一个项目
		管理自己的collabrator
		管理版本和分支
		自己写代码维护自己的开源项目
		review和管理别人提交的pull request
	给别人的开源项目贡献代码
		fork
		pull request
		讨论，尝试说服别人合并自己的代码
		开源项目贡献者

技术梳理和总结
	思维导图总结
	授人以鱼不如授人以渔

### 38、授人以鱼不如授人以渔：如何自己持续学习git技术

是很多同学都给提的一个要求，或者说是一个建议，我平时是怎么学习技术的，然后呢，大家在学习完了课程以后，以后自己对这个技术如何持续学习 

1、我是怎么学习git技术的 

1）找学习资料：中国人写的书，老外的书被翻译，原版的英文书籍，官方文档，技术博客 

第一步：技术博客，你想快速了解和入门一个技术 

百度搜索，***教程，***入门教程，***学习，Git教程 

找一篇或者几篇精品博客，一定是中国人写的，写的是比较精良的，从下载、安装，案例，一步一步有过程的，确保说，你对这个技术，参照这个博客，一步一步做，可以先入个门，一定能做出来，基本掌握一点东西 

告诉大家，看技术博客，只有三个作用：入门；开拓视野；经验总结 

2）深入学习，推荐官方文档 

过一遍，官方文档，100%过了一遍之后，这个技术就到80%了。 

剩下20%，看技术博客，实践经验总结，最重要的，自己在项目中反复实践，通过自己的项目和业务驱动，去考虑如何用这个技术，设计架构，在项目上线之后，积累这个技术实践中遇到的一些问题 

这里就有问题了，有的技术官方文档不是很好的 

比如这个git，就是类似操作手册之类的东西，用于参考，不用于学习

比如activemq，就是很混乱，没有层次感，没有结构感 

还有一个问题，就是官方文档都是纯英文的，就我了解，国内程序员，能无障碍快速阅读英文文档，而且深入透彻理解的，1%，千分之一。一般英语都不太好。 

3）书 

中国人写的 

Git权威指南：这本书我不推荐看，写的很好，但是实在是不太适合学习用

完全学会Git的24堂课：台湾，写的入门，粗糙，没法跟着一步一步做，知识点的总结 

老外写的被翻译了 

5本：翻译的书，翻译的很烂，我就几乎没见过翻译过来的书让我觉得翻译的很好，语句不通，用词尴尬，味如嚼蜡，很枯燥，看不懂 

强烈不推荐看翻译的书 

老外写的原版 

Pro Git（The second edition）：五星推荐，特别棒，最好的一本Git学习的书籍 

所以说推荐看这本书，我当时也是把这本书的第一版和第二版都看过，Pro Git这本书 

不到1%的工程师，我觉得有能力通过读Pro Git这本书透彻了解Git这个技术 

4）到此为止总结一下 

大部分，99%的工程师，对git的学习，主要停留于就是那种技术博客级别的一点简单的了解，对各种书籍，官网，几乎没办法做非常深入的学习，对git的掌握都很浅 

5）光是上面那些不够 

我记得第一次我做直播的时候，有一个同学，问我，老师，你是不是看完官方文档就可以出来讲课？吓人了，当然不是了 

光看完上面那些东西，是不足以讲课的，讲出来的课也是照本宣科，照着官方文档讲 

6）在实践中，在各种项目中，去大量的运用这个技术，设计架构，采坑，积累经验 

7）还不够，此时还差了最后一点点火候，对这个技术，还可以深入的去研究其源码，原理，内幕，底层 

入门（技术博客） + 知识体系（官网/英文原版书） + 大量项目实践（采坑，积累经验） + 深入底层（源码、原理、内幕） 

8）BAT等大的公司出来的架构师，就是很牛，基本上都是走的我这个线路 

快速建立起来一个技术的知识体系：一周之内看完一个技术所有的英文官方文档，或者是原版书籍，大公司硕士以上的名校生学历，英文都很好，国外英国留学回来的，英文是没有问题的 

Pro Git，600页，我一周可以看两遍 

大量项目实践，小公司里技术实践的环境不是太好，大公司里面，核心项目，几十人上百人协作的那种大项目，高并发，高可用，高性能，稳定性，扩展性，重构，基础架构，最新技术的使用，积累国内，行业内顶尖的一手技术经验 

小公司里的同学：不是说每个人都有的机会，尽可能的自己去给自己创造机会i，抓住机会，项目里去实践和积累经验 

深入底层和源码：经过大量的技术研究，技术功底是很深厚；对这门技术之前了解的体系已经很透彻了；在项目里有大量的实践和积累 => 源码，原来如此 => 新增对一门技术的深入理解 

这里有一个问题 

现实问题，技术学习的路径：名校出身的硕士+BAT核心团队工作经历+聪明的头脑，技术可以成长的很快，5年，相当于普通人工作15年都不一定能超过他们 

千千万万普通的同学而言，如何去提高技术呢？ 

1、好好跟着学架构师课程 

这次肯定是用100%最大努力，毫无保留的给大家真的兜底讲出来完整的一个BAT架构师技术体系，真实的大型项目，做到的 

2、高质量的视频课程 

我多年的经验积累，我把整个架构师的技术体系，每一个技术，咬碎嚼烂，讲给大家，对大家是最高效的最好的学习途径 

maven：《maven实战》，断断续续，个把月；几天就看完了

git：《Pro Git》英文书，一两个月才搞完；大白话+画图+接地气，讲课程，一周之内就搞定了 

你可以直接学习到我的技术体系，和我的经验积累，同时你的学习速度可以提高好多倍 

3、自己如何继续去学习技术 

git技术，其实你跟着我的视频学完，看两遍之后，不用去看那些书也可以了 

（1）如果说你想继续去看，同时磨炼自己读英文文档的能力，《Pro Git》英文版，看一遍，慢慢看，提升你的英文文档的学习能力 

（2）git版本不断在变化的，官网 看每个版本的更新的新功能，release notes，看每一个新功能增加了什么东西 

（3）你可能会在公司里去做项目，碰到一些问题：找我请教：跟其他同学交流：百度+google，报错，搜索 

（4）在公司里大量实践git，自己去搭建gitlab服务器，整个项目选择一种合适的开发工作流去做，日常碰到各种git问题和场景，用高阶技巧去处理，自己反复理解git底层的内幕原理 

4、总结一下 

对大家而言 

（1）学习视频课程，尤其是架构师课程：替代你去读官方文档，英文原版书，效率提高很多倍

（2）实践：吸取我的实践经验，你自己多在项目里去用和实践

（3）深入底层：跟着架构师课程走，我会给你去讲解底层，效率会提高很多

（4）直接采取我的学习方法，我觉得对大部分同学不一定合适；但是如果跟着架构师课程走，那么基本可以达到我的学习方法的效果，加上自己也去进行一些技术的积累，完美



 

 

 

 

 

 

 

 

 

 

  

 

 

 

 

 

 

 

 

 

