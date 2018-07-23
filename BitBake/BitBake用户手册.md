<h1 align="center">bitBake用户手册</h1>


## 1. 概述
欢迎使用Bitbake用户手册。本手册提供Bitbake工具的相关信息。这些信息尽可能独立于使用Bitbake的系统，如OpenEmbedded和Yocto Project。在某些情况下，本手册将使用构建系统中的场景或示例来帮助理解。对于这些情况，手册清楚地说明的使用上下文。

### 1.介绍
从根本上说，Bitbake是一个通用任务执行引擎，允许shell和python任务在复杂的任务间依赖约束条件下高效并行运行。Bitbake的主要用户之一，OpenEmbedded，采用Bitbake，并使用面向任务的方法构建嵌入式Linux软件栈。

从概念上讲，Bitbake在某些方面类似于GNU Make，但具有显著差异：

- Bitbake根据提供的构建任务的元数据执行任务。元数据存储在配方（.bb）和相关配方“append”（.bbappend）文件，配置（.conf）和底层包含（.inc）文件以及类（.bbclass）文件中。元数据为Bitbake提供了有关运行哪些任务以及这些任务之间的依赖说明。

- Bitbake包含一个程序库用于从各种地方（如本地文件，源码管理系统或网站）获取源代码。

- 每个要构建的单元（例如一个软件）的指令被称为“配方”文件，并且包含关于该单元的所有信息（依赖性，源文件位置，校验和，描述等扥）。

- Bitbake包含客户端/服务器抽象，可以通过命令行使用或者通过XML-RPC作为服务提供，并具有多个不同的用户界面。

### 1.2.历史和目标
BitBake最初是OpenEmbedded项目的一部分。 它受到了Linux发行版Gentoo使用的Portage包管理系统的启发。2004年12月7日，OpenEmbedded项目组成员克里斯·拉森（Chris Larson）将项目分为两个截然不同的部分：

* BitBake，通用任务执行程序

* OpenEmbedded，由BitBake使用的元数据集

如今，BitBake是OpenEmbedded项目的主要基础，它被用来构建和维护诸如Angstrom之类的Linux发行版，并且也被用作诸如Yocto项目之类的Linux项目的构建工具。

在BitBake之前，没有其他的构建工具能够充分满足有抱负的嵌入式Linux发行版的需求。传统桌面Linux发行版所使用的所有构建系统都缺乏重要的功能，并且在嵌入式领域中普遍使用的基于Buildroot的系统都不具备可扩展性或可维护性。

BitBake最初的一些重要目标包括：

* 处理交叉编译。

* 处理包间依赖（包括在目标架构上构建时的依赖，在本机架构上构建时的依赖及运行时依赖）。

* 支持在给定包中运行任意数量的任务，包括但不限于获取上游源码，解压缩，打补丁，配置等等。

* 对于构建和目标系统，不假设使用什么Linux发行版。

* 不假设使用什么架构。

* 支持多种构建和目标操作系统（如Cygwin，BSD等）。

* 自我包容，而不是紧密集成到生的机器的根文件系统。

* 根据目标架构，操作系统，发行版和机器处理条件元数据。

* 方便使用这些工具来提供本地元数据和包。

* 方便使用BitBake在多个项目之间为其构建进行协作。

* 提供一个继承机制来共享许多包之间的通用元数据。

* 随着时间的推移，还需要进一步的要求：

* 处理基本配方的变体（例如native，sdk和multilib）。

* 将元数据拆分成图层，并允许图层增强或覆盖其他图层。

* 允许将给定的一组输入变量表示为一个校验和。基于该校验和，允许使用预先构建的组件加速构建。

BitBake满足了所有的原始要求，还有更多的扩展是基本的功能来反映附加的要求。灵活性和权力一直是优先事项。BitBake具有高度的可扩展性，支持嵌入式Python代码和执行任意任务。

### 1.3.概念
BitBake是一个用Python语言编写的程序。在最顶层，BitBake解释元数据，决定哪些任务需要运行，并执行这些任务。与GNU Make类似，BitBake控制软件的构建方式。GNU Make通过“makefile”实现控制，而BitBake使用“recipes”。

BitBake扩展了像GNU Make这样的简单工具的功能，允许定义更复杂的任务，比如组装整个嵌入式Linux发行版。

本节的其余部分介绍了为了更好地利用BitBake的功能应该理解的几个概念。

#### 1.3.1. 配方Recipes
以文件扩展名.bb表示的BitBake Recipes是最基本的元数据文件。这些配方文件为BitBake提供了以下内容：

* 关于软件包的描述性信息（作者，主页，许可证等）

* 配方的版本

* 现有的依赖关系（包括构建和运行时依赖关系）

* 源代码驻留在哪里以及如何获取它

* 源代码是否需要任何补丁，在哪里可以找到它们，以及如何应用它们

* 如何配置和编译源代码

* 在目标机器上安装所创建的软件包或软件包的位置

在BitBake或任何使用BitBake作为其构建系统的项目中，具有.bb扩展名的文件被称为配方。

> 注意：“package”一词通常也用来描述配方。但是，由于这个词用来描述项目的打包输出，因此保留一个描述性术语-“recipes”。换句话说，一个单独的“配方”文件相当有能力生成一些相关但可单独安装的“包”。事实上，这种能力是相当普遍的。

#### 1.3.2. 配置文件
配置文件（由.conf扩展名表示）定义了管理项目构建过程的各种配置变量。 这些文件分为几个领域，定义了机器配置选项，分发配置选项，编译器调整选项，通用配置选项和用户配置选项。 主配置文件是位于BitBake源码conf目录中的示例bitbake.conf文件。

#### 1.3.3. 类
类文件（由.bbclass扩展名表示）包含有助于在元数据文件之间共享的信息。BitBake源码当前带有一个名为base.bbclass的类元数据文件。你可以在classes目录下找到这个文件。base.bbclass类文件是特殊的，因为它总是自动包含在所有的配方和类中。这个类包含了标准的基本任务的定义，例如抓取，解包，配置（默认为空），编译（运行任何Makefile），安装（默认为空）和打包（默认为空）。这些任务通常被项目开发过程中添加的其他类所覆盖或扩展。

#### 1.3.4. 层
层允许您隔离不同类型的自定义。虽然在单个项目中工作时，您可能会将所有内容都保存在一个层次上，但是您将元数据组织得越模块化，越容易处理未来的变化。

为了说明如何使用图层来保持模块化，考虑您可能为支持特定目标计算机而进行的自定义。这些类型的自定义通常驻留在特殊层中，而不是称为板级支持包（BSP）层的通用层。此外，例如，机器自定义应该与支持新的GUI环境的配方和元数据隔离。这种情况下你会有几个图层：一个用于机器配置，另一个用于GUI环境。但是，理解BSP层仍然可以在GUI环境层中为机器添加配方，而不会使用特定于机器的更改污染GUI层本身，这一点很重要。您可以通过BitBake附加（.bbappend）文件的配方完成此操作。

#### 1.3.5. 附加文件
附加文件是具有.bbappend文件扩展名的文件，扩展或覆盖现有配方文件中的信息。

BitBake期望每个附加文件都有相应的配方文件。此外，附加文件和相应的配方文件必须使用相同的根文件名。文件名只能在所使用的文件类型后缀上有所不同（例如formfactor_0.0.bb和formfactor_0.0.bbappend）。

附加文件中的信息扩展或覆盖底层的，类似名称的配方文件中的信息。

在命名附加文件时，可以使用通配符（％）来匹配配方名称。例如，假设您有一个如下所示的附加文件：

     busybox_1.21％.bbappend
该附加文件将匹配任何busybox_1.21.x.bb版本的配方。所以，附加文件将匹配以下配方名称：

     busybox_1.21.1.bb
     busybox_1.21.2.bb
     busybox_1.21.3.bb
如果busybox配方更新为busybox_1.3.0.bb，则附加名称不匹配。但是，如果您将追加文件命名为busybox_1.％.bbappend，那么您将有一个匹配项。

在最一般的情况下，你可以命名附加文件像busybox _％.bbappend一样简单，完全独立于版本。

### 1.4.获取Bitbake
您可以通过几种不同的方式获取BitBake：

* **Clone BitBake**：推荐使用Git工具Clone BitBake源代码库是获取BitBake。Clone repo可以很容易地获得错误修复，并有权访问稳定的分支和主分支。一旦你有了BitBake的clone，你应该使用最新的稳定分支进行开发，因为主分支是用于BitBake开发的，并且可能包含不太稳定的变化。

您通常需要与您正在使用的元数据相匹配的BitBake版本。元数据通常是向后兼容的，但不能向前兼容。
以下是一个Clone BitBake Repo的例子：


```bash
    git clone git：//git.openembedded.org/bitbake
```

该命令将BitBake Git Repo Clone到一个名为bitbake的目录中。或者，你也可以在git clone命令后指定一个目录。如下例，命名目录bbdev：

```bash
    git clone git：//git.openembedded.org/bitbake bbdev
```
* 使用分发包管理系统进行安装：不建议使用此方法，因为您的发行版提供的BitBake版本在大多数情况下是在BitBake Repo之前的多个版本。

* 获取BitBake快照：从源代码库下载BitBake的快照，可以访问已知的BitBake分支或版本。

> 注意
> 如前所述，Clone Git Repo是获取BitBake的首选方法。克隆存储库使修补程序添加到稳定分支时更容易更新。

以下示例下载BitBake版本1.17.0的快照：

```bash
    wget http://git.openembedded.org/bitbake/snapshot/bitbake-1.17.0.tar.gz
```
使用tar实用程序解压缩tarball之后，您将拥有一个名为bitbake-1.17.0的目录。

* 使用您的Build Checkout附带的BitBake：获取BitBake副本的最后一个可能性是它已经附带了一个更大的基于Bitbake的构建系统（如Poky或Yocto Project）的checkout。您可以检查出整个构建系统，而不是手动检查各个图层并将它们粘贴在一起。checkout已经包含了一个BitBake的版本，已经过了与其他组件的兼容性测试。有关如何检出特定的基于BitBake的构建系统的信息，请参阅构建系统的支持文档。

### 1.5.Bitbake命令
bitbake命令是BitBake工具的主要接口。 本节介绍BitBake命令语法并提供了几个执行示例。

#### 1.5.1. 用法和语法
以下是Bitbake命令的用法和语法：
```bash

dp@Ubuntu:/home/skunlin/openbmc/build$ bitbake -h
Usage: bitbake [options] [recipename/target recipe:do_task ...]

    Executes the specified task (default is 'build') for a given set of target recipes (.bb files).
    It is assumed there is a conf/bblayers.conf available in cwd or in BBPATH which
    will provide the layer, BBFILES and other configuration information.

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -b BUILDFILE, --buildfile=BUILDFILE
                        Execute tasks from a specific .bb recipe directly.
                        WARNING: Does not handle any dependencies from other
                        recipes.
  -k, --continue        Continue as much as possible after an error. While the
                        target that failed and anything depending on it cannot
                        be built, as much as possible will be built before
                        stopping.
  -a, --tryaltconfigs   Continue with builds by trying to use alternative
                        providers where possible.
  -f, --force           Force the specified targets/task to run (invalidating
                        any existing stamp file).
  -c CMD, --cmd=CMD     Specify the task to execute. The exact options
                        available depend on the metadata. Some examples might
                        be 'compile' or 'populate_sysroot' or 'listtasks' may
                        give a list of the tasks available.
  -C INVALIDATE_STAMP, --clear-stamp=INVALIDATE_STAMP
                        Invalidate the stamp for the specified task such as
                        'compile' and then run the default task for the
                        specified target(s).
  -r PREFILE, --read=PREFILE
                        Read the specified file before bitbake.conf.
  -R POSTFILE, --postread=POSTFILE
                        Read the specified file after bitbake.conf.
  -v, --verbose         Output more log message data to the terminal.
  -D, --debug           Increase the debug level. You can specify this more
                        than once.
  -q, --quiet           Output less log message data to the terminal.
  -n, --dry-run         Don't execute, just go through the motions.
  -S SIGNATURE_HANDLER, --dump-signatures=SIGNATURE_HANDLER
                        Dump out the signature construction information, with
                        no task execution. The SIGNATURE_HANDLER parameter is
                        passed to the handler. Two common values are none and
                        printdiff but the handler may define more/less. none
                        means only dump the signature, printdiff means compare
                        the dumped signature with the cached one.
  -p, --parse-only      Quit after parsing the BB recipes.
  -s, --show-versions   Show current and preferred versions of all recipes.
  -e, --environment     Show the global or per-recipe environment complete
                        with information about where variables were
                        set/changed.
  -g, --graphviz        Save dependency tree information for the specified
                        targets in the dot syntax.
  -I EXTRA_ASSUME_PROVIDED, --ignore-deps=EXTRA_ASSUME_PROVIDED
                        Assume these dependencies don't exist and are already
                        provided (equivalent to ASSUME_PROVIDED). Useful to
                        make dependency graphs more appealing
  -l DEBUG_DOMAINS, --log-domains=DEBUG_DOMAINS
                        Show debug logging for the specified logging domains
  -P, --profile         Profile the command and save reports.
  -u UI, --ui=UI        The user interface to use (depexp, knotty or ncurses -
                        default knotty).
  -t SERVERTYPE, --servertype=SERVERTYPE
                        Choose which server type to use (process or xmlrpc -
                        default process).
  --token=XMLRPCTOKEN   Specify the connection token to be used when
                        connecting to a remote server.
  --revisions-changed   Set the exit code depending on whether upstream
                        floating revisions have changed or not.
  --server-only         Run bitbake without a UI, only starting a server
                        (cooker) process.
  --foreground          Run bitbake server in foreground.
  -B BIND, --bind=BIND  The name/address for the bitbake server to bind to.
  -T IDLE_TIMEOUT, --idle-timeout=IDLE_TIMEOUT
                        Set timeout to unload bitbake server due to inactivity
  --no-setscene         Do not run any setscene tasks. sstate will be ignored
                        and everything needed, built.
  --setscene-only       Only run setscene tasks, don't run any real tasks.
  --remote-server=REMOTE_SERVER
                        Connect to the specified server.
  -m, --kill-server     Terminate the remote server.
  --observe-only        Connect to a server as an observing-only client.
  --status-only         Check the status of the remote bitbake server.
  -w WRITEEVENTLOG, --write-log=WRITEEVENTLOG
                        Writes the event log of the build to a bitbake event
                        json file. Use '' (empty string) to assign the name
                        automatically.
```

#### 1.5.2. 示例
本节介绍一些如何使用BitBake的例子。

##### 1.5.2.1. 针对单个配方执行任务
执行单个配方文件的任务相对简单。您指定有要求的文件，BitBake解析它并执行指定的任务。如果你没有指定一个任务，BitBake会执行默认的任务，即“build”，BitBake在这个过程中服从于任务间的依赖关系。

以下命令在foo_1.0.bb配方文件上运行构建任务（默认任务）：

   bitbake -b foo_1.0.bb

以下命令在foo.bb配方文件上运行clean任务：

   bitbake -b foo.bb -c clean

> 注意：
> `-b`选项显式不处理配方依赖关系。除了出于调试的目的，建议您使用下一节中介绍的语法。

##### 1.5.2.2. 对一组配方文件执行任务
当需要管理多个.bb文件时，会引入一些其他的复杂性。显然，需要有一种方法可以告诉BitBake哪些文件可用，哪些是您想要执行的文件。每个配方还需要有一种方法来表达它的依赖关系，无论是构建时依赖还是运行时依赖。当多个配方提供相同的功能时，或者有多个配方版本时，必须有一种方法可以表达配方首选项。

当不使用“--buildfile”或“-b”时，bitbake命令只接受“PROVIDES”。你不能提供任何其他的东西。默认情况下，配方文件通常“提供”其“包名”，如下所示：

    bitbake foo

下一个示例“提供”包名称，并使用“-c”选项告诉BitBake只执行do_clean任务：

    bitbake -c clean foo
##### 1.5.2.3. 执行任务和配方组合表
当您指定多个目标时，BitBake命令行支持为各个目标指定不同的任务。例如，假设您有两个目标（或配方）myfirstrecipe和mysecondrecipe，并且需要BitBake为第一个配方执行tashA，为第二个配方执行taskB：

```bash
bitbake myfirstrecipe：do_taskA mysecondrecipe：do_taskB
```

##### 1.5.2.4. 生成依赖图
BitBake能够使用dot生成依赖关系图。 您可以使用Graphviz中的dot工具将这些图转换为图片。

当您生成依赖关系图时，BitBake将三个文件写入当前工作目录：

* recipe-depends.dot：显示配方之间的依赖关系（即task-depends.dot的折叠版本）。

* ask-depends.dot：显示任务之间的依赖关系。 这些依赖与BitBake的内部任务执行列表相匹配。

* pn-buildlist：显示要构建的目标的简单列表。

使用“-I”于选项省略指定部分的依赖关系。删除这些信息可以产生更多可读的图表。

以下是创建依赖关系图的两个示例。 第二个例子在OpenEmbedded中常见：

    bitbake -g foo
    bitbake -g -I virtual/kernel -I eglibc foo

## 2. 执行
运行BitBake的主要目的是生成一些输出，比如一个可安装的软件包，一个内核，一个软件开发工具包，甚至一个完整的特定于电路板的可引导的Linux映像，包括引导加载程序，内核和根文件系统。当然，你可以执行带有选项的bitbake命令，这些选项可以执行单个任务，编译单个配方文件，捕获或清除数据，或者简单地返回有关执行环境的信息。

本章介绍BitBake的执行过程，当您使用它来创建image时，从开始到结束。执行过程使用以下命令形式启动：

    $ bitbake target
有关BitBake命令及其选项的信息，请参阅“BitBake命令”部分。

注意：在执行BitBake之前，您应该通过在您的项目的local.conf配置文件中设置BB_NUMBER_THREADS变量来利用您的构建主机上可用的并行线程执行。

为生成主机确定此值的常用方法是运行以下命令：

    $ grep processor / proc / cpuinfo
这个命令返回处理器的数量，这考虑到了超线程。 因此，具有超线程的四核主机很可能会显示八个处理器，这是您将分配给BB_NUMBER_THREADS的值。

一个可能更简单的解决方案是某些Linux发行版（例如Debian和Ubuntu）提供了ncpus命令。

### 2.1.解析基本配置元数据
BitBake做的第一件事是解析基本配置元数据。基本配置元数据由项目的bblayers.conf文件组成，以确定BitBake需要识别的层，所有必需的layer.conf文件（每层一个）以及bitbake.conf。数据本身有各种类型：

* 配方：关于特定的软件的细节。

* 类数据：常见构建信息的抽象（例如，如何构建Linux内核）。

* 配置数据：特定于机器的设置，策略决策等等。配置数据充当了将所有东西绑定在一起的粘合剂。

*layer.conf*文件用于构建关键变量，如*BBPATH*和*BBFILES*。*BBPATH*用于分别在*conf*和*classes*目录下搜索配置和类文件。*BBFILES*用于定位配方和配方附加文件（*.bb*和*.bbappend*）。如果没有*bblayers.conf*文件，则假定用户已经在环境中直接设置了*BBPATH*和*BBFILES*。

接下来，使用刚刚构建的*BBPATH*变量来定位*bitbake.conf*文件。*bitbake.conf*文件也可能包含使用*include*或*require*指令的其他配置文件。

在解析配置文件之前，Bitbake会查看某些变量，包括：

* BB_ENV_WHITELIST
* BB_ENV_EXTRAWHITE
* BB_PRESERVE_ENV
* BB_ORIGENV
* BITBAKE_UI

该列表中的前四个变量与BitBake在任务执行期间如何处理shell环境变量有关。默认情况下，BitBake清理环境变量，并严格控制shell执行环境。但是，通过使用这些前四个变量，您可以在执行任务期间应用您的空间，获取在shell中允许由Bitbake使用的环境变量。请参阅“将信息传递到构建任务环境”一节以及变量术语表中有关这些变量的信息，以获取有关这些变量如何工作以及如何使用它们的更多信息。

基本配置元数据是全局的，因此会影响所有执行的配方和任务。

BitBake首先在当前工作目录中搜索一个可选的conf / bblayers.conf配置文件。这个文件应该包含一个BBLAYERS变量，它是一个空格分隔的“层”目录列表。如果BitBake找不到bblayers.conf文件，那么假定用户已经在环境中直接设置了BBPATH和BBFILES变量。

对于此列表中的每个目录（层），将找到一个conf / layer.conf文件并将LAYERDIR变量传给它，变量值设置为图层所在的目录。目的是这些文件会自动为给定的构建目录设置BBPATH和其他变量。

然后，BitBake希望在用户指定的BBPATH中找到conf / bitbake.conf文件。该配置文件通常包含指令来引入任何其他元数据，例如特定于架构，机器，本地环境等的文件。

在BitBake .conf文件中只允许使用变量定义和包含指令。有些变量直接影响BitBake的行为。这些变量可能是根据前面提到的或在配置文件中设置的环境变量从环境中设置的。“变量术语”一章介绍了变量的完整列表。

解析配置文件后，BitBake使用其基本的继承机制，通过类文件，继承一些标准的类。当遇到负责获取那个类的继承指令时，BitBake解析一个类。

base.bbclass文件始终包含在内。还包括使用INHERIT变量在配置中指定的其他类。BitBake以与配置文件相同的方式在BBPATH中的路径下的类子目录中搜索类文件。

了解执行环境中使用的配置文件和类文件的一个好方法是运行以下BitBake命令：

    $ bitbake -e> mybb.log</pre>
检查mybb.log的顶部将显示执行环境中使用的许多配置文件和类文件。

注意：您需要了解BitBake如何分析花括号。 如果一个配方在函数中使用了一个大括号，并且该字符没有前导空格，BitBake会产生解析错误。 如果在外壳功能中使用一对花括号，则不能在没有前导空格的行的起始位置放置关闭的大括号。

这是一个导致BitBake产生解析错误的例子：

```python
 fakeroot create_shar() {
 cat << "EOF" > ${SDK_DEPLOY}/${TOOLCHAIN_OUTPUTNAME}.sh
 usage()
 {
 echo "test"
 ###### The following "}" at the start of the line causes a parsing error ######
 }
 EOF
 }
用这种方法写配方避免了这个错误：

 fakeroot create_shar() {
 cat << "EOF" > ${SDK_DEPLOY}/${TOOLCHAIN_OUTPUTNAME}.sh
 usage()
 {
 echo "test"
 ######The following "}" with a leading space at the start of the line avoids the error ######
 }
 EOF
 }
 ```
### 2.2.查找和分析配方
在配置阶段，BitBake将会设置BBFILES。BitBake现在使用它来构建一个解析的配方列表，以及所有使用的附加文件（.bbappend）。BBFILES是可用文件的空格分隔列表，并支持通配符。一个例子如下：

BBFILES =“/path/to/bbfiles/*.bb /path/to/appends/*.bbappend”
BitBake解析BBFILES中的每个配方及附加文件，并将各种变量的值存储到数据存储中。

附加文件按照在BBFILES中出现的顺序应用。

对于每个文件，创建基本配置的全新副本，然后逐行分析配方。任何继承语句都会触发BitBake使用BBPATH作为搜索路径来查找并解析类文件（.bbclass）。最后，BitBake按顺序解析在BBFILES中找到的任何附加文件。

一个常见的约定是使用配方文件名来定义元数据。例如，在bitbake.conf中，配方名称和版本用于设置变量PN和PV：

    PN = "${@bb.parse.BBHandler.vars_from_file(d.getVar('FILE', False),d)[0] or 'defaultpkgname'}"
    PV = "${@bb.parse.BBHandler.vars_from_file(d.getVar('FILE', False),d)[1] or '1.0'}"
在这个例子中，一个名为“something_1.2.3.bb”的配方会将PN设置为“something”，将PV设置为“1.2.3”。

在配方完成解析之后，BitBake拥有配方定义的任务列表，以及由键和值组成的一组数据以及有关任务的相关性信息。

BitBake不需要所有这些信息。它只需要一小部分信息就可以决定配方。因此，BitBake会缓存它感兴趣的值，而不会保存其余的信息。经验表明，重新解析元数据要比试图将其写入磁盘然后重新加载更快。

在可能的情况下，后续的BitBake命令会重新使用这个配方信息缓存。首先计算基本配置数据的校验和（请参阅BB_HASHCONFIG_WHITELIST），然后检查校验和是否匹配，从而确定此缓存的有效性。如果该校验和与缓存中的内容匹配并且配方和类文件没有更改，则Bitbake可以使用该缓存。然后BitBake重新加载有关配方的缓存信息，而不是从头开始重新解析。

配方文件集合的存在使得用户可以拥有包含相同的确切软件包的.bb文件的多个存储库。例如，可以很容易地使用它们来创建上游存储库的本地副本，但可以使用自定义修改，而不需要上游。一个例子如下：

    BBFILES = "/stuff/openembedded/*/*.bb /stuff/openembedded.modified/*/*.bb"
    BBFILE_COLLECTIONS = "upstream local"
    BBFILE_PATTERN_upstream = "^/stuff/openembedded/"
    BBFILE_PATTERN_local = "^/stuff/openembedded.modified/"
    BBFILE_PRIORITY_upstream = "5"
    BBFILE_PRIORITY_local = "10"
注意：图层机制现在是收集代码的首选方法。 虽然集合代码仍然存在，但其主要用途是设置图层优先级并处理图层之间的重叠（冲突）。

### 2.3.Providers
假设BitBake已经被指示执行一个目标，并且所有的配方文件已经被解析，BitBake开始计算出如何构建目标。BitBake通过PROVIDES列表查看每个配方。PROVIDES列表是可以知道配方的名称列表。每个配方的PROVIDES列表通过配方的PN变量隐式创建，并通过配方的PROVIDES变量显式创建，该变量是可选的。

当配方使用PROVIDES时，该配方的功能可以在隐式PN名称以外的其他名称下找到。例如，假设名为keyboard_1.0.bb的配方包含以下内容：

PROVIDES += "fullkeyboard"
这个配方的PROVIDES列表变成了“键盘”，这是隐式的，“fullkeyboard”是显示的。因此，可以在两个不同的名称下找到keyboard_1.0.bb中的功能。

### 2.4.Preferences
PROVIDES列表只是解决目标配方的解决方案的一部分。由于目标可能有多个providers，因此BitBake需要通过确定providers偏好来确定providers的优先级。

目标具有多个provider程序的常见示例是“virtual/kernel”，位于每个内核配方的PROVIDERS列表上。每台机器通常通过在机器配置文件中使用与以下内容类似的一行来选择最佳的内核提供者：

PREFERRED_PROVIDER_virtual / kernel =“linux-yocto”
默认的PREFERRED_PROVIDER是与目标名称相同的provider。Bitbake遍历每个需要构建的目标，并使用这个过程解决它们及其依赖关系。

由于一个给定的provider可能存在多个版本，如何选择provider就变得复杂了。BitBake默认为provider的最高版本。版本比较使用与Debian相同的方法进行。您可以使用PREFERRED_VERSION变量来指定特定的版本。您可以通过使用DEFAULT_PREFERENCE变量影响顺序。

默认情况下，文件的优先级为“0”。将DEFAULT_PREFERENCE设置为“-1”将使得配方不太可能被使用到，除非它被明确引用。将DEFAULT_PREFERENCE设置为“1”可能会使用配方。 PREFERRED_VERSION将覆盖任何DEFAULT_PREFERENCE设置。 DEFAULT_PREFERENCE通常用于标记更新，更多实验测试的配方版本，直到经过足够的测试才能被认为是稳定的。

当给定配方有多个“版本”时，除非另有说明，BitBake默认选择最新的版本。如果所讨论的配方的DEFAULT_PREFERENCE设置低于其他配方（默认值为0），则不会被选中。这允许维护配方文件存储库的人员指定他们对默认选定版本的偏好。另外，用户可以指定他们的首选版本。

如果第一个配方命名为a_1.1.bb，则PN变量将被设置为“a”，并且PV变量将被设置为1.1。

因此，如果存在名为a_1.2.bb的配方，BitBake将默认选择1.2。但是，如果您在BitBake分析的.conf文件中定义以下变量，则可以更改该首选项：

PREFERRED_VERSION_a =“1.1”
注意：一个配方通常提供两个版本 - 一个稳定的，编号（和首选）的版本，以及一个从源代码库自动检出的版本，这个版本被认为是更为“bleeding edge”，但只能显式选择。

例如，在OpenEmbedded代码库中，BusyBox有一个标准版本配方文件busybox_1.22.1.bb，但也有一个基于Git的版本busybox_git.bb，它明确包含了行

     DEFAULT_PREFERENCE =“-1”
以确保编号的稳定版本始终是首选的，除非开发者另有选择。

### 2.5.依赖
每个目标BitBake构建包括多个任务，如提取，解包，修补，配置和编译。为了在多核系统上获得最佳性能，BitBake将每个任务视为一个独立的实体，并拥有自己的一套依赖关系。

依赖性通过几个变量来定义。您可以在本手册末尾的变量词汇表中找到有关变量BitBake使用的信息。在基本级别上，知道BitBake在计算依赖关系时使用DEPENDS和RDEPENDS变量就足够了。

有关BitBake如何处理依赖关系的更多信息，请参阅“依赖项”部分。

### 2.6.任务列表
基于生成的provider列表和依赖信息，BitBake现在可以准确计算出需要运行的任务以及按照什么顺序运行它们。“执行任务”部分有更多关于BitBake如何选择接下来要执行的任务的信息。

现在，构建以BitBake fork线程开始，最多可以fork的线程数目由BB_NUMBER_THREADS变量设置。只要有任务准备好运行，这些任务的所有依赖关系都已满足，并且线程数目阈值没有被超过，BitBake就继续fork线程。

值得注意的是，通过正确设置BB_NUMBER_THREADS变量，可以大大加快编译时间。

当每个任务完成时，时间戳被写入由STAMP变量指定的目录。在后续运行中，BitBake会在tmp/ stamps内的构建目录中查找，并且不会重新运行已完成的任务，除非发现时间戳无效。目前，仅在每个配方文件的基础上考虑无效的时间戳。因此，例如，如果配置戳记的时间戳大于给定目标的编译时间戳，那么编译任务将重新运行。但是，再次运行编译任务对依赖于该目标的其他提供程序没有影响。

时间戳的确切格式部分是可配置的。在现代版本的BitBake中，hash被附加到stamp上，以便如果配置更改，stamp将变为无效，并且任务会自动重新运行。此hash或使用的签名由配置的签名策略管理（有关信息，请参阅“Checksums（签名）”部分）。也可以使用[stamp-extra-info]任务标志将额外的元数据附加到stamp上。例如，OpenEmbedded使用这个标志来完成一些特定于机器的任务。

有些任务被标记为“nostamp”。这些任务运行时不会创建时间戳文件，因此，执行构建任务时，“nostamp”总是重新运行。

更多详细信息，请参阅“Task”章节描述。

### 2.7.执行任务
任务可以是shell或Python任务。对于shell任务，BitBake将shell脚本写入$ {T} /run.do_taskname.pid，然后执行脚本。生成的shell脚本包含所有导出的变量，并且shell函数包含所有变量。shell脚本的输出转到文件$ {T} /log.do_taskname.pid。查看运行文件中的扩展shell函数和日志文件中的输出是一种有用的调试技术。

对于Python任务，BitBake在内部执行任务并将信息记录到控制终端。未来版本的BitBake将把函数写入类似于shell任务处理方式的文件中。日志记录将以类似于shell任务的方式进行处理。

BitBake运行任务的顺序由其任务调度程序控制。可以配置调度程序并为特定用例定义自定义实现。有关更多信息，请参阅控制行为的这些变量：

BB_SCHEDULER
BB_SCHEDULERS
可以在任务的main函数之前和之后运行某些函数。这是通过使用列出要运行的函数的任务的[prefuncs]和[postfuncs]标志完成的。

### 2.8.校验
校验和是任务输入的唯一签名。任务的签名可以用来确定任务是否需要运行。因为任务输入的变化触发了任务的运行，所以BitBake需要检测给定任务的所有输入。对于shell任务，事实证明这是相当简单的，因为BitBake为每个任务生成一个“运行”shell脚本，并且可以创建一个校验和，让你很好地了解任务数据的变化。

全面点分析，有些事情不应该包含在校验和中。首先，给定任务的实际特定构建路径-工作目录。工作目录是否改变并不重要，因为它不应该影响目标包的输出。排除工作目录的简单方法是将其设置为某个固定值，并为“运行”脚本创建校验和。BitBake更进一步，使用BB_HASHBASE_WHITELIST变量来定义生成签名时不应包含的变量列表。

另一个问题是由包含可能被调用或可能不被调用的函数的“运行”脚本产生的。增量构建解决方案包含计算shell函数之间依赖关系的代码。这段代码被用来修剪“运行”脚本到最小集合，从而缓解这个问题，并使“运行”脚本更加可读。

到目前为止，我们解决shell脚本任务。那么Python的任务呢？即使这些任务比较困难，同样的方法也适用。该过程需要确定一个Python函数访问什么变量以及它调用哪些函数。同样，增量构建解决方案包含首先计算变量和函数依赖关系的代码，然后为用作输入任务的数据创建校验和。

像工作目录一样，应该忽略依赖关系的情况。对于这些情况，可以通过使用如下所示的行来指示构建过程忽略依赖关系：

PACKAGE_ARCHS[vardepsexclude] = "MACHINE"

作者：半天妖
链接：https://www.jianshu.com/p/5a1c26839dfe
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。