## 第1章Yocto项目开发手册


### 1.1。介绍
欢迎来到Yocto项目开发手册！本手册提供了如何使用Yocto项目开发在目标设备上运行的嵌入式Linux映像和用户空间应用程序的信息。该手册使用Yocto项目提供了镜像，内核和用户空间应用程序开发的概述。由于本手册中的大部分信息都是通用的，因此它包含许多对其他来源的引用，您可以在其中找到更多详细信息。例如，您可以在Internet上的许多位置找到有关Git，存储库和开源的详细信息。Yocto项目的另一个例子是如何快速设置主机开发系统和构建镜像， [Yocto项目快速入门]()。

但是，Yocto项目开发手册提供了有关如何使用流行的Eclipse ™IDE 更改内核源代码，重新配置内核以及开发应用程序的指导和示例。

> 注意
> 默认情况下，使用Yocto项目创建Poky发布版。但是，您可以通过提供关键元数据来创建自己的发布版 。一个很好的例子是Angstrom，它自Yocto项目成立以来就已经有了一个基于Yocto项目的发行版。其他例子包括商业发行版如 Wind River Linux, Mentor Embedded Linux, ENEA Linux 和其他。有关详细信息，请参阅“ [创建自己的发布版]() ”部分。
### 1.2。本手册提供的内容
以下列表描述了您可以从本手册中获得的内容：

* 允许您设置使用Yocto项目进行开发的信息。

* 帮助开源环境新手的开发人员以及Yocto Project使用的分布式版本控制系统Git的信息。

* 了解常见的端到端开发模型和任务。

* 有关嵌入式设备图像开发过程中常用的常见开发任务的信息。

* 有关使用QuickEMUlator（QEMU）的Yocto Project集成的信息，它允许您模拟在硬件上运行使用OpenEmbedded构建系统构建的映像。

* 许多参考其他相关信息来源。
### 1.3。本手册未提供的内容
本手册不会给您以下内容：

* **这些说明的分步说明在其他Yocto Project文档中：** 例如，Yocto Project Application Developer's Guide包含有关如何运行 [ADT安装程序的详细说明]()，该[安装程序]()用于设置交叉开发环境。

* **参考资料：** 此类材料存在于适当的参考手册中。例如，系统变量记录在 [Yocto项目参考手册]()中。

* **非Yocto项目特有的详细公共信息：** 例如，有关如何使用Git的详尽信息通过Internet比本手册更好。
### 1.4。其他信息
由于本手册提供了许多不同主题的概述信息，因此建议使用补充信息以便完全理解。以下列表显示了您可能会发现有用的其他信息来源：

**[Yocto项目网站]()：** [Yocto项目]()的主页提供了有关项目的大量信息以及软件和文档的链接。

**[Yocto项目快速入门]()：** 这个简短的文档让您可以开始使用Yocto项目并快速开始构建图像。

**[Yocto项目参考手册]()：** 本手册是OpenEmbedded构建系统的参考指南，它基于BitBake。构建系统有时被称为“Poky”。

**[Yocto项目应用程序开发人员指南]()：** 本指南提供的信息可让您使用应用程序开发工具包（ADT）和独立的交叉开发工具链来使用Yocto项目开发项目。

**[Yocto项目板支持包（BSP）开发人员指南]()：** 本指南定义了BSP组件的结构。具有通常理解的结构鼓励标准化。

**[Yocto Project Linux内核开发手册]()：** 本手册介绍了如何使用Linux Yocto内核，并提供了有关Yocto Linux内核树构造的一些概念信息。

**[Yocto项目概要分析和跟踪手册]()：** 本手册介绍了一组常用且通用的跟踪和概要分析方案及其应用程序（视情况而定）。

**[Toaster用户手册]()**： 本手册介绍并介绍了如何设置和使用Toaster，它是Yocto Project的OpenEmbedded Build System的Web界面 。

**[Eclipse IDE Yocto插件]()**： 逐步说明的视频，演示应用程序开发人员如何在Eclipse IDE中使用Yocto插件功能。

**[常见问题]()：**常见问题 列表及其答案。

**[发行说明]()：** Yocto项目当前版本的功能，更新和已知问题。

**[Hob]()：** BitBake的图形用户界面。Hob的主要目标是使用户能够更轻松地执行常见任务。

**[Toaster]()：** OpenEmbedded构建系统的应用程序编程接口（API）和基于Web的接口，它使用BitBake报告构建信息。

**[构建设备]()：** 一种虚拟机，使您可以使用非Linux开发系统使用Yocto项目构建和引导自定义嵌入式Linux映像。

**[Bugzilla]()：** Yocto项目使用的bug跟踪应用程序。如果您发现Yocto项目存在问题，则应使用此应用程序报告它们。

**[Yocto项目邮件列表]()：** 要订阅Yocto项目邮件列表，请单击以下URL并按照说明操作：

* http://lists.yoctoproject.org/listinfo/yocto 获取Yocto项目讨论邮件列表。

* http://lists.yoctoproject.org/listinfo/poky 有关OpenEmbedded构建系统（Poky）的Yocto Project Discussions邮件列表。

* http://lists.yoctoproject.org/listinfo/yocto-announce 获取邮件列表，以接收官方Yocto项目公告以及Yocto项目里程碑。

* http://lists.yoctoproject.org/listinfo 列出了所有公共邮件列表 lists.yoctoproject.org。

**[互联网中继聊天（IRC）]()：** freenode上的两个IRC频道可用于Yocto项目和Poky讨论：#yocto和 #poky。

**[OpenEmbedded]()：** Yocto项目使用的构建系统。该项目是上游的，通用的嵌入式发行版，Yocto项目从中派生出构建系统（Poky）并为其贡献。

**[BitBake]()：** OpenEmbedded构建系统用于处理项目元数据的工具。

**[BitBake用户手册]()：** BitBake工具的综合指南。如果您需要有关BitBake的信息，请参阅本手册。

**[Quick EMUlator（QEMU）]()：** 一个开源机器模拟器和虚拟器。

## 第2章Yocto项目入门

本章介绍Yocto项目，并让您了解入门所需的内容。通过阅读Yocto项目快速入门，您可以找到足够的信息来设置开发主机，并为Yocto项目支持的硬件构建或使用图像 。

本章的其余部分总结了Yocto项目快速入门中的内容，并提供了您可能需要考虑的一些更高级别的概念。

### 2.1。介绍Yocto项目
Yocto项目是一个专注于嵌入式Linux开发的开源协作项目。该项目目前提供了一个构建系统， 在Yocto项目文档中称为 OpenEmbedded构建系统。Yocto项目为嵌入式开发人员提供了各种辅助工具，还具有Sato参考用户界面，该界面针对手写笔驱动的低分辨率屏幕进行了优化。

您可以使用OpenEmbedded构建系统（使用 BitBake）为基于ARM，MIPS，PowerPC，x86和x86-64的体系结构开发完整的Linux映像和相关的用户空间应用程序。

> 注意
> 默认情况下，使用Yocto项目创建Poky分发。但是，您可以通过提供关键元数据来创建自己的分发 。有关详细信息，请参阅“ 创建自己的分发 ”部分。
虽然Yocto项目没有提供严格的测试框架，但它确实为您提供或生成可以执行目标级别和模拟测试和调试的工件。此外，如果您是Eclipse ™IDE用户，则可以安装Eclipse Yocto插件，以便在熟悉的环境中进行开发。

### 2.2。设置
以下是使用Yocto项目所需的内容：

* **主机系统：** 你应该有一个合理前面引用的相同 wiki页面介绍了如何设置 meta-intelGit存储库。

Eclipse Yocto插件： 如果您使用Eclipse集成开发环境（IDE）开发应用程序，则需要此插件。有关更多信息，请参阅“设置Eclipse IDE ”部分。基于Linux的主机系统。最近发布的Fedora，openSUSE，Debian，Ubuntu或CentOS将获得最佳结果，因为这些版本经常针对Yocto项目进行测试并获得官方支持。有关验证中的分发及其状态的列表，请参阅Yocto项目参考手册中的“支持的Linux发行版 ”部分和分发支持中的Wiki页面 。

您还应该有大约50 GB的可用磁盘空间来构建映像。

* **包：** OpenEmbedded构建系统要求开发系统上存在某些包（例如Python 2.7）。请参阅“[Yocto项目快速启动的package]()”“[部分所需包的主机开发系统]() ”的[Yocto项目参考手册]()中节的具体包装要求和安装命令来安装它们所支持的分布。

* **Yocto项目发布：** 您需要在开发系统上本地安装Yocto项目的版本。该文档将这组本地安装的文件称为源目录。您可以使用 Git克隆上游poky存储库的本地副本，或者通过下载和解压缩官方Yocto Project版本的tar包来创建源目录 。首选方法是创建存储库的克隆。

通过上游存储库的副本，您可以回馈Yocto项目，或者只使用开发分支上的最新软件。由于Git维护并创建了具有完整更改历史记录的上游存储库，并且您正在使用该存储库的本地克隆，因此您可以访问上游存储库中使用的所有Yocto Project开发分支和标记名称。

> 注意
> 您可以在<http://git.yoctoproject.org/cgit.cgi>上查看Yocto项目源存储库

以下脚本显示了如何将pokyGit存储库克隆 到当前工作目录中。该命令在名为的目录中创建本地存储库poky。有关Yocto项目中使用的Git的信息，请参阅“ Git ”部分。

```bash
     $ git clone git://git.yoctoproject.org/poky
     Cloning into 'poky'...
     remote: Counting objects: 226790, done.
     remote: Compressing objects: 100% (57465/57465), done.
     remote: Total 226790 (delta 165212), reused 225887 (delta 164327)
     Receiving objects: 100% (226790/226790), 100.98 MiB | 263 KiB/s, done.
     Resolving deltas: 100% (165212/165212), done
```

有关如何建立自己的本地Git仓库另一个例子，看到这个 wiki页面，介绍如何创建本地Git仓库 poky和meta-intel。

* **Yocto项目内核：** 如果要对受支持的Yocto项目内核进行修改，则需要建立源的本地副本。您可以在 http://git.yoctoproject.org/cgit.cgi 的Yocto项目源存储库中的“Yocto Linux Kernel”下找到受支持的Yocto Project内核的Git存储库 。

此设置可能涉及创建Yocto Project内核的裸克隆，然后复制该克隆的存储库。您可以在任何您喜欢的地方创建裸克隆和裸克隆的副本。为简单起见，建议您在源目录之外创建这些结构，通常将其命名poky。

作为示例，以下脚本显示如何创建linux-yocto-3.19内核的裸克隆，然后创建该克隆的副本。

> 注意
> 当您拥有本地Yocto Project内核Git存储库时，您可以引用该存储库而不是上游Git存储库作为clone命令的一部分。这样做可以加快这个过程。


     $ git clone --bare git://git.yoctoproject.org/linux-yocto-3.19 linux-yocto-3.19.git
     Cloning into bare repository 'linux-yocto-3.19.git'...
     remote: Counting objects: 3983256, done.
     remote: Compressing objects: 100% (605006/605006), done.
     remote: Total 3983256 (delta 3352832), reused 3974503 (delta 3344079)
     Receiving objects: 100% (3983256/3983256), 843.66 MiB | 1.07 MiB/s, done.
     Resolving deltas: 100% (3352832/3352832), done.
     Checking connectivity... done.

现在创建刚刚创建的裸克隆的克隆：

     $ git clone linux-yocto-3.19.git my-linux-yocto-3.19-work
     Cloning into 'my-linux-yocto-3.19-work'...
     done.
     Checking out files: 100% (48440/48440), done.

* **在*meta-yocto-kernel-extras*Git仓库：** 在meta-yocto-kernel-extrasGit仓库包含元数据，只需要如果要修改和建立内核映像。特别是，它包含内核BitBake append（.bbappend）文件，您编辑这些文件以指向本地修改的内核源文件并构建内核映像。指向这些本地文件比每次更改内核时从上游下载内核的源文件要高效得多。

您可以meta-yocto-kernel-extras在http://git.yoctoproject.org/cgit.cgi 的Yocto项目源存储库的“Yocto元数据层”区域中找到Git存储库 。最好在源目录中创建此Git存储库。

以下是meta-yocto-kernel-extras在源目录中创建Git存储库的示例，poky 在此情况下命名：

     $ cd ~/poky
     $ git clone git://git.yoctoproject.org/meta-yocto-kernel-extras meta-yocto-kernel-extras
     Cloning into 'meta-yocto-kernel-extras'...
     remote: Counting objects: 727, done.
     remote: Compressing objects: 100% (452/452), done.
     remote: Total 727 (delta 260), reused 719 (delta 252)
     Receiving objects: 100% (727/727), 536.36 KiB | 240 KiB/s, done.
     Resolving deltas: 100% (260/260), done.

* **支持的板级支持包（BSP）：** Yocto项目支持许多BSP，这些BSP维护在各自的层或层中，旨在包含多个BSP。要通过BSP层了解机器支持，您可以查看 该版本的[机器索引]()。

Yocto项目使用以下BSP层命名方案：
    meta-bsp_name

bsp_name是公认的BSP名称。这里有些例子：
     meta-crownbay
     meta-emenlow
     meta-raspberrypi

有关BSP层的更多信息，请[参阅Yocto项目板支持包（BSP）开发人员指南]()中的“ BSP层 ”部分。

与Yocto项目一起发布的有用的Git存储库meta-intel是一个包含许多支持的BSP层的父层 。您可以meta-intel在http://git.yoctoproject.org/cgit.cgi 的Yocto项目源存储库的“Yocto元数据层”区域中找到Git存储库 。

如果您正在使用BSP，使用 Git创建上游存储库的本地克隆可能会有所帮助。通常，您meta-intel 在源目录中设置Git存储库。例如，以下脚本显示了要克隆的步骤 meta-intel。

注意
务必在meta-intel 与源目录 （即poky）分支匹配的 分支中工作。例如，如果您已经签出了“master”分支，poky并且您将要使用 meta-intel，请务必签出“master”分支meta-intel。
     $ cd~ / poky
     $ git clone git：//git.yoctoproject.org/meta-intel.git
     克隆成'meta-intel'......
     remote：计数对象：8844，完成。
     remote：压缩对象：100％（2864/2864），完成。
     远程：总计8844（delta 4931），重用8780（delta 4867）
     接收物体：100％（8844/8844），2.48 MiB | 264 KiB / s，完成了。
     解决增量：100％（4931/4931），完成。

前面引用的相同 wiki页面介绍了如何设置 meta-intelGit存储库。

* **Eclipse Yocto插件：** 如果您使用Eclipse集成开发环境（IDE）开发应用程序，则需要此插件。有关更多信息，请参阅“设置Eclipse IDE ”部分。

### 2.3。建筑图像
构建过程从源创建整个Linux发行版，包括工具链。有关此主题的更多信息，请参阅Yocto项目快速入门中的“ [构建映像]() ”部分。

构建过程如下：

* 确保已设置上一节中描述的源目录。

* 通过获取构建环境脚本（即oe-init-build-env 或 oe-init-build-env-memres）来初始化构建环境 。

* （可选）确保conf/local.conf在构建目录中找到的配置文件按 您希望的方式设置。此文件定义了构建环境的许多方面，包括通过MACHINE变量的目标机器体系结构 ，build（PACKAGE_CLASSES）期间使用的打包格式，以及通过DL_DIR变量的集中式tarball下载目录 。

* 使用bitbake命令构建映像。如果您需要有关BitBake的信息，请参阅 [BitBake用户手册]()。

* 在实际硬件上或使用QEMU仿真器运行映像。

### 2.4。使用预建的二进制文件和QEMU 
您必须开始的另一个选择是使用预先构建的二进制文件。Yocto项目为每个版本提供了许多类型的二进制文件。有关Yocto Project版本附带的二进制文件类型的说明，请参阅Yocto项目参考手册中的“ [图像]() ”一章。

使用预构建的二进制文件非常适合开发在目标硬件上运行的软件应用程序。为此，您需要能够为正在开发的体系结构访问相应的跨工具链tarball。如果您使用的是SDK类型图像，则该图像附带了该体系结构的完整工具链。如果您不使用SDK类型映像，则需要单独下载并安装独立的Yocto Project跨工具链tarball。

无论您使用何种类型的映像，都需要下载将在QEMU仿真器中启动的预构建内核，然后下载并提取目标计算机体系结构的目标根文件系统。您可以从计算机获取特定于体系结构的二进制文件和文件系统 。您可以从工具链获取独立工具链的安装脚本 。获得所有文件后，可以通过获取环境设置脚本来设置环境以模拟硬件。最后，启动QEMU仿真器。您可以在“ 使用预建二进制文件和QEMU ”中找到有关所有这些步骤的详细信息“Yocto项目快速入门部分。您可以在” 使用快速EMUlator（QEMU） “部分了解有关在Yocto项目中使用QEMU的更多信息。

使用QEMU模拟硬件可能会导致速度问题，具体取决于目标和主机架构组合。例如，qemux86在基于Intel的32位（x86）主机上使用模拟器中的映像很快，因为目标和主机架构匹配。另一方面，qemuarm在同一台基于Intel的主机上使用图像可能会更慢。但是，您仍然可以忠实地模拟ARM特定的问题。

为了加快速度，QEMU映像支持使用distcc 在仿真系统外调用交叉编译器。如果您曾经runqemu启动过QEMU，并且 distccd应用程序存在于主机系统上，那么构建系统中提供的任何BitBake交叉编译工具链都可以通过调用从QEMU中自动使用distcc。您可以通过定义交叉编译器变量（例如export CC="distcc"）来实现此目的。或者，如果您使用合适的SDK映像或存在相应的独立工具链，则还会自动使用工具链。

> 注意
> 有几种机制可以让您连接到QEMU仿真器上运行的系统：
> * QEMU提供了一个帧缓冲接口，可以使标准控制台可用。
> 
> * 通常，无头嵌入式设备具有串行端口。如果是这样，您可以配置正在运行的映像的操作系统以使用该端口来运行控制台。该连接使用标准IP网络。
> 
> * 某些QEMU映像中存在SSH服务器。该core-image-satoQEMU图像具有与禁用根密码运行Dropbear安全外壳（SSH）服务器。在core-image-full-cmdline和 core-image-lsbQEMU图像有OpenSSH的，而不是Dropbear。包括这些SSH服务器允许您使用标准 ssh和scp命令。该core-image-minimalQEMU的形象，但是，不包含SSH服务器。
> 
> * 您可以使用提供的用户空间NFS服务器使用主机上根文件系统的本地副本来引导QEMU会话。要建立此连接，必须使用该runqemu-extract-sdk命令提取根文件系统tarball 。运行该命令后，必须将runqemu 脚本指向解压缩的目录而不是根文件系统映像文件。


## 第3章Yocto项目开源开发环境 

### 3.1。开源哲学
开源哲学的特点是通过活跃的开发人员社区，通过同行生产和协作进行软件开发。将此与商业软件公司使用的更标准的集中式开发模型进行对比，其中有限的开发人员使用一组定义的程序生产销售产品，最终产生其结构和源材料对公众不公开的最终产品。

开源项目在概念上具有不同的并发议程，方法和生产。开发过程的这些方面可以来自与软件项目有利害关系的公众（社区）中的任何人。开源环境包含与更传统的开发环境不同的新版权，许可，域和消费者问题。在开源环境中，最终产品，源材料和文档都可以免费向公众开放。

开源项目的一个基准示例是Linux内核，最初由芬兰计算机科学专业学生Linus Torvalds于1991年构思和创建。相反，非开源项目的一个很好的例子是开发的 Windows®系列操作系统由Microsoft®Corporation提供。

维基百科在这里对开源哲学有着良好的历史描述 。您还可以在此处找到有关如何参与Linux社区的有用信息 。

### 3.2。在团队环境中使用Yocto项目
可能不会立即清楚如何在团队环境中使用Yocto项目，或者为大型开发人员团队扩展它。Yocto项目的优势之一是它非常灵活。因此，您可以将其适应许多不同的用例和场景。但是，如果您尝试创建可扩展到大型团队的工作设置，这些特征可能会导致困难。

为了帮助解决这些类型的情况，本节介绍了项目中最成功的一些经验，实践，解决方案和可用的技术。请记住，这里的信息是一个起点。您可以构建它并对其进行自定义以适应任何特定的工作环境和一组实践。

#### 3.2.1。系统配置
跨大型团队的系统应该满足两类开发人员的需求：那些从事操作系统映像本身内容和开发应用程序的人员。无论开发人员的类型如何，他们的工作站都必须具有相当强大的功能并运行Linux。

##### 3.2.1.1。应用程序开发
对于主要在现有软件堆栈上执行应用程序级别工作的开发人员，以下是一些最有效的实践：

使用包含软件堆栈本身的预构建工具链。然后，在堆栈顶部开发应用程序代码。这种方法适用于少量相对隔离的应用程序。

如果可能，请使用Eclipse ™IDE 的Yocto Project插件和其他应用程序开发技术（ADT）。有关更多信息，请参阅“ 应用程序开发工作流程 ”部分以及 Yocto项目应用程序开发人员指南。

保持交叉开发工具链的更新。您可以通过配置新工具链下载或通过包更新机制进行更新opkg 来提供，以便为现有工具链提供更新。如何以及何时执行此操作的确切机制是本地政策的问题。

使用本地安装在不同位置的多个工具链，以允许跨版本进行开发。

##### 3.2.1.2。核心系统开发¶
对于核心系统开发，通常最好在开发人员工作站上提供构建系统本身，以便开发人员可以运行自己的构建并直接重建软件堆栈。您应该尽可能保持核心系统不变，并在核心系统之上分层工作。这样做可以在升级到新版本的核心系统或板级支持包（BSP）时提供更高级别的可移植性。您可以在特定项目的开发人员之间共享层，并包含定义项目的策略配置。

除了以前的最佳实践，还有许多提示和技巧可以帮助加快核心开发项目：

* 在快速网络上的开发人员组中使用 共享状态缓存（sstate）。共享sstate的最佳方式是通过网络文件系统（NFS）共享。第一次构建给定组件的第一个用户将该对象贡献给sstate，而来自其他开发人员的后续构建则重用该对象而不是自己重建它。

* 尽管可以为sstate使用其他协议（如HTTP和FTP），但应避免使用这些协议。使用HTTP将sstate限制为只读，FTP提供的性能较差。

* 与开发人员工作站的贡献类似，autobuilders有助于sstate池。有关信息，请参阅“ Autobuilders ”部分。

* 如果由于某种原因开发人员工作站不满足最低系统要求（如最新的Python版本chrpath或其他工具），请构建包含“缺少”系统要求的独立tarball 。您可以像通常的交叉开发工具链一样安装和重新定位tarball，以便所有开发人员都能满足大多数发行版的最低版本要求。

* 使用少量共享的高性能系统进行测试（例如，具有24 GB RAM和大量磁盘空间的双核，六核Xeon）。开发人员可以使用这些系统进行更广泛，更广泛的测试，同时继续使用其主要开发系统在本地开发。

* 当包裹馈送需要增量并且PR 值不断增加时，启用PR服务 。通常，当您使用或发布程序包源并使用共享状态时，会出现这种情况。您应该为使用共享状态池的所有用户启用PR服务。有关PR服务的更多信息，请参阅“ 使用PR服务 ”。

#### 3.2.2。源控制管理（SCM）¶
建议您 在与OpenEmbedded构建系统兼容的SCM系统的控制下保持 元数据和您正在开发的任何软件。在BitBake支持的SCM中，Yocto项目团队强烈建议使用 Git。Git是一个易于备份的分布式系统，允许您远程工作，然后连接回基础架构。

注意
有关BitBake的信息，请参阅 BitBake用户手册。
设置Git服务和创建http://git.yoctoproject.org这样的基础设施相对容易 ，它基于被调用的服务器软件 gitolite，cgit 用于生成允许您查看存储库的Web界面。该gitolite软件使用SSH密钥识别用户，并允许基于分支的访问控制到您可以根据需要尽可能少地控制的存储库。

注意
这些服务的设置超出了本手册的范围。但是，存在这些描述如何执行设置的站点：
Git文档：描述如何gitolite 在服务器上安装。

在gitolite主索引：为所有主题gitolite。

接口，前端和工具：有关如何为Git创建接口和前端的文档。

#### 3.2.3。Autobuilders ¶
Autobuilders通常是开发项目的核心。在这里，来自各个开发人员的变更汇集在一起​​并进行集中测试，并且可以进行后续的发布决策。Autobuilders还允许软件组件的“持续集成”样式测试以及回归识别和跟踪。

有关更多信息和buildbot的链接，请参阅“ Yocto Project Autobuilder ”。Yocto项目团队发现这个实现在这个角色中运作良好。一个公开的例子是Yocto项目Autobuilders，我们用它来测试项目的整体健康状况。

该系统的特点是：

* 提交破坏构建时的亮点。

* 填充sstate缓存，开发人员可以从中提取而不需要本地构建。

* 允许提交钩子触发器，在提交时触发构建。

* 允许在QuickEMUlator（QEMU）下触发自动图像引导和测试。

* 支持增量构建测试和从头开始构建。

* 允许开发人员测试和历史回归调查的股票输出。

* 创建可用于发布的输出。

* 允许调度构建，以便有效地使用资源。

#### 3.2.4。政策和变更流程
Yocto项目本身使用分层结构和拉模型。存在用于创建和发送拉取请求（即create-pull-request和 send-pull-request）的脚本。此模型与其他开源项目一致，其中维护者负责项目的特定区域，并且单个维护者处理最终的“树顶”合并。

> 注意
> 您还可以使用更集体的推模型。该gitolite软件非常容易支持推拉模型。

与任何开发环境一样，重要的是记录所使用的策略以及任何主要项目指南，以便每个人都能理解它们。拥有结构良好的提交消息也是一个好主意，这通常是项目指南的一部分。回顾过去并尝试理解为何进行更改时，良好的提交消息是必不可少的。
 
如果您发现项目的核心层需要进行更改，则应尽快与社区分享这些更改。如果您发现需要更改，社区中的其他人也需要更改。

#### 3.2.5。摘要¶
本节总结了前几节中介绍的主要建议：

* 使用Git 作为源控制系统。

* 在对您的情况有意义的层中维护您的元数据。有关图层的详细信息，请参阅“ 了解和创建图层 ”部分。

* 使用单独的Git存储库分离项目的元数据和代码。有关这些存储库的信息，请参阅“ Yocto项目源存储库 ”部分。有关如何为相关的上游Yocto Project Git存储库设置本地Git存储库的信息，请参阅“ 获取设置 ”部分。

* 为SSTATE_DIR有意义的共享状态缓存（）设置目录。例如，在同一组织中的开发人员使用的系统上设置sstate缓存，并在其计算机上共享相同的源目录。

* 设置一个Autobuilder并让它填充sstate缓存和源目录。

* Yocto项目社区鼓励您向项目发送修补程序以修复错误或添加功能。如果确实提交了修补程序，请按照项目提交指南编写好的提交消息。请参阅“ 如何提交更改 ”部分。

* 稍后将更改发送到核心，因为其他人可能会遇到相同的问题。有关要使用的邮件列表的一些指导，请参阅“ 如何提交更改 ”部分中的列表。有关可用邮件列表的说明，请参阅Yocto项目参考手册中的“ 邮件列表 ”部分。

### 3.3。Yocto项目源存储库
Yocto项目团队在http://git.yoctoproject.org/cgit/cgit.cgi上为所有Yocto项目文件维护完整的源代码库 。这个基于Web的源代码浏览器按功能组织成类别，例如IDE Plugins，Matchbox，Poky，Yocto Linux Kernel等。在界面中，您可以单击“名称”列中的任何特定项目，并查看页面底部的URL，您需要克隆该特定项目的Git存储库。拥有源目录的本地Git存储库 ，通常命名为“poky”，允许您进行更改，为历史做出贡献，并最终增强Yocto项目的工具，Board Support Packages等。

对于任何受支持的Yocto Project版本，您还可以访问 Yocto项目网站并选择“下载”选项卡，并获取已发布的poky存储库tarball 或任何受支持的BSP tarball。解压缩这些tarball可以为您提供已发布文件的快照。

> 笔记
> 
> 设置Yocto项目源目录的推荐方法 和支持的BSP（例如meta-intel）的文件是使用 Git创建上游存储库的本地副本。
> 
> 确保始终在所选BSP存储库和源目录 （即poky）存储库的匹配分支中工作 。例如，如果您已经签出了“master”分支，poky并且您将要使用 meta-intel，请务必签出“master”分支meta-intel。

总之，在这里您可以获得开发所需的项目文件：

* **源存储库：** 此区域包含IDE插件，Matchbox，Poky，Poky支持，工具，Yocto Linux内核和Yocto元数据层。您可以为每个区域创建Git存储库的本地副本。
![](https://www.yoctoproject.org/docs/1.8/dev-manual/figures/source-repos.png)

* **索引/版本：** 这是版本的索引，例如Eclipse™Yocto插件，杂项支持，Poky，Pseudo，交叉开发工具链的安装程序，以及图像或tarball形式的Yocto Project的所有发布版本。下载和提取这些文件不会生成Git存储库的本地副本，而是生成特定版本或映像的快照。
![](https://www.yoctoproject.org/docs/1.8/dev-manual/figures/index-downloads.png)

Yocto项目网站的 “下载”页面 ： 访问该网页，然后选择“下载”选项卡。此页面允许您以tarball形式下载任何Yocto Project版本或Board Support Package（BSP）。tarball类似于 Index of / releases：区域中的tarball 。
![](https://www.yoctoproject.org/docs/1.8/dev-manual/figures/yp-download.png)