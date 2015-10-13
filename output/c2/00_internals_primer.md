> 翻译：[koffuxu](https://github.com/koffuxu)
> 校对：

# 内部入门 

正如我们所看到的那样，Android源码能够非常自由的下载，修改，并安装在你选择的设备上。事实上，获得源码，编译它，然后在模拟器上运行这是相当简单的。然而，按照你的设备和硬件，客制化AOSP，需要你对Android内部构件有一定程度的了解。所以在这章，我们将以较高的视角来理解Android内部架构，以便在接下来的章节，更深层次的挖掘这些内部部分，包括试图讲清楚真正的AOSP的内部细节。

`此书是基本Android2.3/Gingerbread来讨论的。尽管在Android的整个生命周期中，就写该书之前，其内部构架是相当稳定的，但由于Android的封闭开发，一些关键的改变也没有宣告出来。比如说，在2.2/Froyo以前的版本（含2.2/Froyo），状态栏是System Server的一部分，但在2.3/Gingerbread，状态栏已经从System Server中移出来成为单独的进程。