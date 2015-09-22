---
layout: appendix
title: 第3讲附录 FFmpeg
dsq_id: l03
---

视频中提到了， `dwave` 命令不支持 .mp3 格式的音频文件，需要用转码软件转成 .wav 或者 .ogg 格式。

那么具体该怎么做呢？

首先，我们需要一个转码软件，有很多种类。像付费的 [MediaEspresso](http://www.cyberlink.com/products/mediaespresso/) ，但是我想大部分人都不愿意花钱吧？（[盗版可耻！](http://www.microsoft.com/china/genuine/consumer/mistakes.aspx)）

至于免费的，你可能已经听说过[格式工厂](http://www.pcfreetime.com/CN/index.html)了，它看起来是个十分优秀的软件，但实际上，我要告诉你一个非常不幸的消息——它的底层是 FFmpeg ，格式工厂只是做了个界面而已。

[FFmpeg](http://ffmpeg.org/) 是一个[自由软件](http://www.gnu.org/philosophy/free-sw.html)，但是格式工厂使用了 FFmpeg 的东西却[没有遵守其协议](https://zh.wikipedia.org/wiki/%E6%A0%BC%E5%BC%8F%E5%B7%A5%E5%8E%82#.E7.89.88.E6.AC.8A.E7.88.AD.E8.AD.B0 "打不开？用后面这个链接凑合着看吧")（[参考资料备用链接](http://baike.baidu.com/view/1820476.htm#6 "虽然这个链接可以凑合看，但是如果你有时间，还是最好参考一下页面底部的链接")），这是非常可耻的。

所以我们可以直接使用 FFmpeg ，这样不但可以有效地遏制代码盗用的现象，还能够获得更大的灵活性。但是有一个缺点，使用方法 **看起来** 似乎很难——这就是我写这个页面的原因。

------------------

首先先下载。但通常情况下 **不是** 官网上那个最大的“ Download ”按钮。

Linux 请使用各自的包管理器安装， Debian 系的包名是 `libav-tools` ，其他的是 `ffmpeg` 。

Windows 请访问[这里](http://ffmpeg.zeranoe.com/builds/)，按照[你的电脑是64位还是32位](https://support.microsoft.com/en-us/kb/827218/zh-cn)，点击 "Download FFmpeg git-\*\*\*\*\*\*\* 32-bit/64-bit **Static** " 按钮。

Mac OS X 可以从[这里](http://evermeet.cx/ffmpeg/)下载并把解压出来的文件拷到 `/usr/bin` 里，也可以使用 [HomeBrew](http://brew.sh/) 或 [MacPorts](https://www.macports.org/) 安装。

------------------

然后你需要运行 FFmpeg ，别惊讶，他是一个在[命令行界面](https://zh.wikipedia.org/wiki/%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2)运行的程序，是没有[图形化界面](https://zh.wikipedia.org/wiki/%E5%9B%BE%E5%BD%A2%E7%94%A8%E6%88%B7%E7%95%8C%E9%9D%A2)的。

Linux 和 OS X 直接打开终端输入 `ffmpeg` （ Debian 系是 `avconv` ）。 Windows 在解压后运行 `ff-prompt.bat` ，出来一个[黑黑的窗口]({{ site.baseurl }}/image/ffmpeg/ff-prompt.png)，别害怕，这是正常的。

然后cd到你要转换的文件所在的目录，比如我把文件放在了 `C:\Users\Alex\Music` 里面，就运行 `cd C:\Users\Alex\Music` 然后回车，注意如果路径包含空格，请用半角引号把 **路径** 包起来。  
![查看文件路径]({{ site.baseurl }}/image/ffmpeg/directory-01.png)
![复制]({{ site.baseurl }}/image/ffmpeg/directory-02.png)
![粘贴]({{ site.baseurl }}/image/ffmpeg/directory-03.png)
![按回车]({{ site.baseurl }}/image/ffmpeg/directory-04.png)
![完毕]({{ site.baseurl }}/image/ffmpeg/directory-05.png)  
**注意：** 如果你使用的是 Windows ，且目标目录跟 FFmpeg 所处的地方不在一个盘符，需要 **再“运行”** 一下盘符。比如目标目录是 D 盘，则运行 `d:` （大小写无关）。

然后运行 `ls` （ OS X 和 Linux ）或 `dir` （ Windows ）来查看当前目录的文件，如果输出的是你想要来到的目录（在我这儿是 `C:\Users\Alex\Music` ）中的文件，则 `cd` 成功了。  
![运行 dir 的结果]({{ site.baseurl }}/image/ffmpeg/dir.png)

然后开始转码，我这里以将 `btn16.mp3` 这个音频文件转换为 `libvorbis` 编码，并在同目录下保存为 `btn16.ogg` 作为例子。

先输入 `ffmpeg`  ，但别着急按回车，右边还要输入别的东西。  
因为我们的输入文件叫做 `btn16.mp3` ，所以在后面加上 `-i btn16.mp3` ，注意如果文件名包含空格，请用双引号把 **文件名** 包起来。  
因为我们要把它的音频流（因为这是音频文件，只有个音频流，如果是视频就不一样了）转换为 `libvorbis` 编码，就加上 `-c:a libvorbis` ~~， c 代表 codec ， a 代表 audio~~ 。  
因为我们要把转换后的文件保存为 `btn16.ogg` ，所以在 **最后** 加上 `btn16.ogg` ，注意如果文件名包含空格，请用双引号把文件名包起来。

总的来说，我们要运行的命令是 `ffmpeg -i btn16.mp3 -c:a libvorbis btn16.ogg` 。（别把我的句号也写进去啊！）  
![在命令行中输入这一条命令]({{ site.baseurl }}/image/ffmpeg/co.png)

然后按下回车，因为文件很小，所以一秒钟不到就完成了。  
你的输出可能跟我不一样，但只要没出现红色的 `ERROR` 之类的字样，一般就是没有问题地完成了。  
![运行，完成]({{ site.baseurl }}/image/ffmpeg/proc.png)

如果你接下来不需要再转换别的文件，那么就可以直接关掉窗口，然后你会发现原来的地方多了个叫 `btn16.ogg` 的文件。大多数音乐播放器都可以播放它（ Windows Media Player 不算）。

一切正常的话，把它复制到你的游戏目录（或游戏目录中的目录）里面，可以继续写脚本了！

-------------------

如果你发现在本页中的某些外部链接无法被打开，请参考[这个页面]({{ site.baseurl }}/cant-open.html)。
