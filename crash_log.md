# crash log的获取
当你的app 在手机上crash的时候，会在手机上自动生成一个崩溃日志，也就是我们说的Crash Log。
CrashLog的位置位于:
iPhone设备的var/mobile/Library/Logs/CrashReporter
我们要获取的就是设备中的这个CrashLog。

## 获取用户的 crash log
>注意。这里的用户指的是你的app已经上架到AppStore上后的用户。

作为开发者，你想要获取到你的用户的崩溃日志的话就得通过 iTunes Connect 了。在 iTunes Connect 上的 Manage Your Applications -> View Details -> Crash Reports

>这种方式有个前提，就是用户设备同意上传相关信息，打开了诊断与用量这个选项设置->隐私->诊断与用量 （由于笔者还未有app上架，所以这个方法笔者未用过，so 就此打住。 希望有用过的大牛来拍砖或者补充，Thx）

## 获取测试机的crash log

很多测试人员在测试途中，或者开发者在自测的途中，会遇到APP crash的情况。

一般的bug，一个合格的测试可以给出明确的重现步骤让开发者清晰地知道bug原因；

也有不少bug，很多时候是偶现的，很可能无法再次重现出来，无法重现出来的bug是开发者头疼的，测试一般会给出bug的截图和重现步骤；

而一般crash是比较严重的问题了（所以千万不能当什么都没发生过，不然会被打的233），这个时候崩溃日志就尤为重要了，把崩溃日志send给开发人员，如此才能让开发者快速定位到错误的原因和位置。

那么测试如何拿到crash日志呢？

>方法一：连接电脑，通过iTools高级选项来获取崩溃日志（Mac版的找不到高级选项T.T，望赐教补充）

>方法二：连接电脑，去本地目录找
Mac : ~/Library/Logs/CrashReporter/MobileDevice/<DEVICE_NAME>

Windows : C://Users/<USERNAME>/AppDataRoamingApple/ComputerLogsCrashReporterMobileDevice/<DEVICE_NAME>/

这个时候你会发现一大堆的.crash文件和.ips文件


>方法三：通过Xcode获取到崩溃日志，方法是Xcode->Window->Devices

# Crash Log的符号化   
获取到了.crash或者.ips文件的时候(憋纠结这两个文件有什么差，改下后缀名就ok)，用文本编辑器打开文件是一堆十六进制的内存地址，你会郁闷的发现压根看不懂。

> Q:十六进制内存地址可以改成看得懂的么？   

A：当然，将这些十六进制地址转化成方法名称和行数的过程称之为Symbolication（符号化）。符号化很简单，只要你把你的.crash文件拉到上面提到过的Xcode的device log里面，然后几秒钟后就会符号化。但是这里有个前提，就是这个发生crash的版本包必须是你自己的Xcode里面Archive出来的（这个是苹果自带的方法，会自动检测是否含有匹配的.dSYM文件和应用二进制文件）。

> Q：那如果要是在新电脑上也想符号化怎么办？

答案是，只有相匹配的.dSYM文件和应用二进制文件就可以符号化。必需完全匹配才行。否则，日志将无法被完全符号化。

上图是.dSYM文件的位置，应用的二进制文件就是打的包得.ipa后缀改成.zip，然后解压后里面有个.app文件就是应用的二进制文件。
将.dSYM文件与.app文件 和crash文件放一个目录下，然后再用deviceLog方法就可以符号化了。
另外还有另外符号化iOS Crash文件的3种方法有大牛已经整合得非常好了，给个链接，这里就不赘述了。
符号化以后是这样的~

这样看上去就倍儿爽了^_^

# Crash Log的分析
接下来就让我们对已经符号化以后的crash文件进行分析。
网上已有的分类比较多，我这里直接把我目前一般找crash原因的模块展示出来，其他的就留待各位自己去研究了，分别是设备和crash信息、异常信息、线程信息
1、首先是设备和crash信息

```
Incident Identifier: F3573A...E2F244A              //crash的id
CrashReporter Key:   cc2298...es77eeb              //crash的设备id
Hardware Model:      iPhone7,2                     //手机型号
Process:             [AppName] [1816]              //APP的名字[进程的id]
Path:                /private/.../Application...   //APP的位置
Identifier:          com....                       //bundle ID
Version:             14 (2.3.5)                    //版本号
Code Type:           ARM-64 (Native)               //app的应用架构之类不大清楚，^_^
Parent Process:      launchd [1]

Date/Time:           2015-10-26 15:03:29.29 +0800    //crash发生时间
Launch Time:         2015-10-26 14:58:28.28 +0800    //进入应用时间
OS Version:          iOS 9.1 (13B143)                //iOS版本
Report Version:      105
```

当你有大量的crash文件的时候，你就可以对crash文件里面的 Hardware Model，Version ， OS Version等进行分类，就可以获知到很多信息，比如说，你会知道crash一般发生原因是因为手机型号，还是App版本，或者还是手机版本的原因。（笔者暂时没碰过大量的crash文件，所以只能纸上谈兵了^_^）

2、其次是异常信息

```
Exception Type:  EXC_BAD_ACCESS (SIGABRT)                      //异常的类型
Exception Subtype: KERN_INVALID_ADDRESS at 0x0000000000000118  //异常子类型
Triggered by Thread:  0                    //异常发生的线程(0为主线程，其他为子线程)
```

3、线程信息

```
Last Exception Backtrace:
0   CoreFoundation                    0x182780f48 __exceptionPreprocess + 124
1   libobjc.A.dylib                   0x197333f80 objc_exception_throw + 56
2   CoreFoundation                    0x182780e90 +[NSException raise:format:] + 120
3   [AppName]                            0x100c42a40 UmengSignalHandler + 144
4   libsystem_platform.dylib          0x197d6193c _sigtramp + 52
5   [AppName]                            0x1005d9f38 CScopePtr<IAVGAudioLogic>::operator IAVGAudioLogic*<IAVGAudioLogic>() (xprefc.h:165)
6   [AppName]                            0x1005d3b8c tencent::av::AVRoomMultiImpl::GetAudioLogic() (av_room_multi_impl.h:119)
7   [AppName]                            0x10057076c tencent::av::AVAudioCtrlImpl::SetAudioOutputMode(int) (av_audio_ctrl_impl.cpp:443)
8   [AppName]                            0x10044dc3c -[AVBasicManager changeSpeakerMode:] (AVManager.mm:525)
9   [AppName]                            0x100296e1c -[KTQAVRoom enableSpeakerMode:] (KTQAVRoom.m:345)
10  [AppName]                            0x1002970d0 -[KTQAVRoom settingSpeaker:] (KTQAVRoom.m:362)
11  [AppName]                            0x1003d5464 -[KTChatView onAudioNotificationReceived:] (KTChatView.m:685)
```

恩。。。这符号化以后应该可以看懂了吧，这个crash的问题应该是腾讯第三方的一个冲突吧233

一般来说，通过异常信息和线程信息就可以找到crash的原因了。

## 补充一些异常类型信息

> 这里参考了很多信息，有很多的异常类型，有些没遇到过，这里就厚颜摘抄过来了（这里是原文地址：iOS Crash文件的解析，再次感谢大牛们的经验）

1、Exception Type
1）EXC_BAD_ACCESS

> 此类型的Excpetion是我们最长碰到的Crash，通常用于访问了不改访问的内存导致。一般EXC_BAD_ACCESS后面的"()"还会带有补充信息。

SIGSEGV: 通常由于重复释放对象导致，这种类型在切换了ARC以后应该已经很少见到了。
SIGABRT: 收到Abort信号退出，通常Foundation库中的容器为了保护状态正常会做一些检测，例如插入nil到数组中等会遇到此类错误。
SEGV:（Segmentation Violation），代表无效内存地址，比如空指针，未初始化指针，栈溢出等；
SIGBUS：总线错误，与 SIGSEGV 不同的是，SIGSEGV 访问的是无效地址，而 SIGBUS 访问的是有效地址，但总线访问异常(如地址对齐问题)
SIGILL：尝试执行非法的指令，可能不被识别或者没有权限

2）EXC_BAD_INSTRUCTION

> 此类异常通常由于线程执行非法指令导致
3）EXC_ARITHMETIC

> 除零错误会抛出此类异常

2、Exception Code
0xbaaaaaad 此种类型的log意味着该Crash log并非一个真正的Crash，它仅仅只是包含了整个系统某一时刻的运行状态。通常可以通过同时按Home键和音量键，可能由于用户不小心触发
0xbad22222 当VOIP程序在后台太过频繁的激活时，系统可能会终止此类程序
0x8badf00d 程序启动或者恢复时间过长被watch dog终止
0xc00010ff 程序执行大量耗费CPU和GPU的运算，导致设备过热，触发系统过热保护被系统终止
0xdead10cc 程序退到后台时还占用系统资源，如通讯录被系统终止
0xdeadfa11 前面也提到过，程序无响应用户强制关闭

# 总结
最后总结一些可能会对各位有用的博文：
1、iOS应用崩溃日志分析(这最后有一个栗子很有意思)
2、获取 iOS crash log(分析得很详细)
3、WWDC视频(2010年的WWDC视频)
4、官网文档——Analyzing iOS Application Crash Reports
