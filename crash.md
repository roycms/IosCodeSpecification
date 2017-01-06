# crash 崩溃查看

> 参考 http://www.jianshu.com/p/e428501ff278    
> 总结作者 https://github.com/ZHThinker   
> 我们每次打包产品交付给测试部门时，通常会运行archive编译输出，输出后会产生.app 和 .dSYM文件，我们会将.app交付给测试部门。       
> 测试部门在测试时遇到崩溃，通常会给我们IPS为扩展名的崩溃日志文件，我们应该怎样查看这个文件呢？      

## 第一步  
* 准备一个 symbolicatecrash 文件
* symbolicatecrash文件地址命令：
```
find /Applications/Xcode.app -name symbolicatecrash -type f 
```
运行后输出的是symbolicatecrash文件的地址，复制 .symbolicatecrash 文件出来到桌面crash文件夹内,地址是：  /Users/zhang/Desktop/crash/ 

## 第二步  
* 将.app和.dSYM以及.symbolicatecrash全部都放到一个文件夹 /Users/zhang/Desktop/crash/ 将IPS文件更该扩展名为.crash也存放在一起
* 如果以下4个文件都准备好了之后  
.app、  .crash、    .dSYM、    .symbolicatecrash

## 第三步 
* 执行（注意路径要根据自己mac 的情况进行修改，文件名字也要修改）
```
./symbolicatecrash /Users/xxx/Desktop/crash/IXQ-2017-01-05-170207.crash /Users/xxx/Desktop/crash/IXQ.app.dSYM > Control_symbol.crash
```

* 如果命令执行后报错 
```
Error: "DEVELOPER_DIR" is not defined at /usr/local/bin/symbolicatecrash line 53
```
* 执行以下命令： 
```
export DEVELOPER_DIR="/Applications/Xcode.app/Contents/Developer"
```
## 第四步 

* 再次执行4，会得到一个Control_symbol.crash 文件，此文件为crash 日志文件，可查看详细的崩溃信息
