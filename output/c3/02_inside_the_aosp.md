> 翻译：[koffuxu](https://github.com/koffuxu)

> 校对：

# AOSP内部

现在，我们已经下载下来了AOSP，就让我们来看看这个里面有些什么，最重要的是，与我们前面章节提到的内容建议联系。如果你很渴望知道你自己客户Android的运行，建议你路过这节，当时完成后面的章节后回过来看。如果你仍然阅读，让我们来看看下面这张介绍AOSP顶层目录的表格3-1。

*表格3-1 AOSP 内容总揽*

| 目录 | 内容 | 大小（MB） |
| -- | -- | -- |
| *bionic* | Android的客户C库 | 14 |
| *bootable* | 涉及bootloader和recovery机制 | 4 |
| *build* | 编译系统 | 4 |
| *cts* | 兼容性测试 | 78 |
| *dalvik* | Dalvik虚拟机 | 35 |
| *development* | 开发工具 | 65 |
| *device* | 设备相关的文件和模块 | 17 |
| *external* | AOSP使用的外部项目 | 854 |
| *frameworks* | 诸如system services之类的核心组件 | 361 |
| *hardware* | HAL和硬件支持库 | 27 |
| *libcore* | Apache Harmony库 | 54 |
| *ndk* | 本地开发工具箱 | 13 |
| *packages* | Android的应用程序，providers和输入法 | 117 |
| *prebuilt* | 预编译工具，包括工具链 | 1,389 |
| *sdk* | 软件开发工具 | 14 |
| *system* | Android的“嵌入式Linux”平台 | 31 |

正如你所见，*prebuild*和*external*是目录树中最大的两个文件夹，接近整个代码的75%。有趣的是，这两个文件夹是由开放源码项目组成的，它们包括各种GNU版本的工具链，Kernel镜像文件，通用库和像OpenSSl和WebKit等等之类的框架层。*libcore*也是另外一个开放源码项目，Apache Harmony。本质上，这个进一步证明，Android是一个高度依赖于其它开源生涯项目而存在的系统。但，Android仍然包含了许多“原生”（或接近“原生”）的代码；大约有800MB。

为了更好的理解Android的源码，回头参考前面章节[图 2-1]()是非常有用的，它说明了Android的构架。[图 3-1]()是从另外一个角度说明在AOSP源码中各个组件的结构。明显地，许多关键的组件是来自*framework/base*这个文件夹，它相当于Android的“大脑”枢纽。事实上，这个目录是值得仔细阅读的，在[表 3-2]()和[表 3-3]()分别包含了大量移动块，这些作为一个嵌入式的开发者也许会感兴趣。

*表 3-2 frameworks/base/内容总揽*

| 目录 | 内容 |
| -- | -- |
| *cmds* | 本地命令和精灵进程 |
| *core* | 以Andoird.*开头的包 |
| *data* | 字体和声音 |
| *graphics* | 2D图形和渲染脚本 |
| *include* | C语言头文件 |
| *keystore* | 安全密钥存储 |
| *libs* | C库 |
| *location* | 本地provider |
| *media* | 媒体服务，StageFright，编解码等待 |
| *native* | 一些框架层组件的本地代码 |
| *obex* | 蓝牙Obex |
| *opengl* | OpenGL库和Java代码 |
| *packages* | 一些像状态栏那校报核心包 |
| *services* | 系统服务 |
| *telephony* | 电话API |
| *tools* | 一些像*aapt*和*aidl*的核心工具 |
| *voip* | RTP和SIP的API |
| *vpn* | VPN管理 |
| *wifi* | Wifi管理和API |

***

*表 3-3 system/core/内容总揽*

| 目录 | 内容 |
| -- | -- |
| *adb* | ADB的守护进程和客户端 |
| *cpio* | 用于生成RAM镜像的*mkbootfs*工具[^b] |
| *debuggerd* | [第2章]()涉及到的*debuggerd*命令 |
| *fastboot* | fastboot是使用“fastboot”协议与Android bootloaders通信的工具 |
| *include* | 整个系统的C语言头文件 |
| *init* | Android的*init* |
| *libacc* | “几乎”是C的编译库用于编译类C代码；用于渲染[^c] |
| *libcutils* | 不属于标准C库的各种C工具函数；用于整个目录 |
| *libdiskconfig* | 用于读取和配置磁盘，用于*vold* |
| *liblinenoise* | BSD_licesed readline()的替代 [*http://github.com/antirez/linenoise*]()；用于Android的Shell |
| *liblog* | 如[图2-2]()所示是与Android内核Logger模块的接口和库；用  于整个目录 |
| *libmincrypt* | 基础的RSA和SHA功能；用于recovery机制和*mkbootimg*工具 |
| *libnetutils* | 网络配置库；用于*netd* |
| *libpixdlfinger* | 底层的图形渲染功能 |
| *libsysutils* | 系统各个组件通信的工具，用于*netd*和*vold* |
| *libzipfile* | 打包zlib用于处理zip文件 |
| *logcat* | *logcat*工具 |
| *logwrapper* | Android的Logger系统中，用于标准输出和标准错误输出重定向传递命令参数的工具 |
| *mkbootimg* | 使用RAM Disk和Kernel制作一个启动镜像 |
| *netcfg* | 网络配置工具 |
| *rootdir* | 默认的Android根目录结构和内容 |
| *run-as* | 按照指定的用户ID运行指定程序的工具 |
| *sh* | Android Shell |
| *toolbox* | Android的工具箱（BusyBox替代） |


还有，*base/* ,*frameworkds/*包含了其它的目录，但是他们没有*base/*基础，同样的，除了*core/*,*system/*也包含了一些像*netd/*和*vold/*的目录，这些分别包含了*netd*和*vold*的守护进程。

另外，在顶层目录中，根目录包含了一个Makefle文件。这个文件几乎是空，这个主要是包含用于Android编译系统的入口：

```
### DO NOT EDIT THIS FILE ###
include build/core/main.mk
### DO NOT EDIT THIS FILE ###

```

正如你已经知道，这与我提供与你的AOSP理解相距甚远。毕竟，在2.3.x/Gingerbread有超过14,000目录和100,000文件。按标准来说，这已经是一个很大的工程了。相比之下，基于释放版的3.0.x版本Linux内核有2,000个目录和35,000文件。随着我们的推进，我们将有机会探索AOSP的源码。我强烈建议，尽管，你早前已经开始探索和经历整个源码，你可以轻松的以自己的方式来查看源码。
