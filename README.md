#DOL开发环境配置

@(开发环境)[分布式|DOL|Markdown]
 
- **Description ** ： DOL 框架描述；
- **How to install ** ：DOL 安装笔记；
- **Experimental experience ** ：实验感想、实验心得:

-------------------

[TOC]

## DOL框架描述

分布式操作层（DOL）是一个能使应用（半）自动映射到多处理器结构平台的框架。DOL由三个基础部分组成：

#### API接口（DOL Application Programming Interface）
DOL定义了一系列计算和传输的程序接口，使分布式程序、图形平台上的并行应用成为了可能。用这些程序接口，应用程序开发者可以在不用知道底层详细的内容的情况下进行开发。事实上，这些程序接口 在基于硬件的软件层上 被进一步细化 。

#### 模拟功能（DOL Functional Simulation）
为了给开发者提供测试应用的功能，DOL开发出来了模拟功能框架。除了模拟测试应用程序之外，这个架构还被用在获取应用级性能参数上。

#### DOL映射优化（DOL Mapping Optimization）
DOL映射的优化目的在于要计算一组从应用程序到图形结构平台的最优映射。在第一步，基于XML，描述应用和简述级的结构的规格协议 已经被定义。然而，所有用来获取准确性能估计的必要信息是包含的。



##  DOL 安装笔记 

#### Ubuntu环境工具简介
 Make工具简介
* 在Linux和Ubuntu环境中，make工具主要被用来进行工程编译和程序链接
* Makefile文件：告诉make以何种方式编译源代码和链接程序
* make通过比较对应文件（规则的目标和依赖）的最后修改时间，来决定哪些文件需要更新、那些文件不需要更新。

 Ant工具简介
* Ant是一种基于Java的build工具。
* Ant用Java的类来扩展。
* Ant本身就是这样一个流程脚本引擎，用于自动化调用程序完成项目的编译，打包，测试等。

 
#### 1.安装必要的环境

 安装一些必要的环境(ubuntu为例)：
```
 $ sudo apt-get update
 $ sudo apt-get install ant
 $ sudo apt-get install openjdk-7-jdk
 $ sudo apt-get install unzip
```

#### 2.下载文件
```
sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz

sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
```
#### 3.解压文件

新建dol的文件夹 
```
$	mkdir dol
```
将dolethz.zip解压到 dol文件夹中
```
$	unzip dol_ethz.zip -d dol
```
解压systemc
```
$	tar -zxvf systemc-2.3.1.tgz
```


#### 4.编译systemc
解压后进入systemc-2.3.1的目录下
```
$	cd systemc-2.3.1
```
新建一个临时文件夹objdir
```
$	mkdir objdir
```
进入该文件夹objdir
```
$	cd objdir
```
运行configure(能根据系统的环境设置一下参数，用于编译)
```
$	../configure CXX=g++ --disable-async-updates
```
运行configure之后
![Alt text](http://i1.piimg.com/4851/1b537caece48621f.png)

编译 , 编译完后文件目录如下

```
$	sudo make install
$ cd ..        $ ls
```
![Alt text](http://i1.piimg.com/4851/325c016ad4ce64c0.png)

记录当前的工作路径(会输出当前所在路径，记下来，待会有用)
```
$	pwd
```
![Alt text](http://i1.piimg.com/4851/b7538d135f393fe9.png)
这里表示我当前的工作路径为 /root/systemc-2.3.1

#### 5.编译DOL

进入刚刚dol的文件夹
```
$	cd ../dol
```
修改build_zip.xml文件
找到下面这段话，就是说上面编译的systemc位置在哪里，
` <property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>`
把 ***YYY*** 改成上页pwd的结果（注意，对于  64位 系统的机器，lib-linux要改成lib-linux64）

然后是编译，若成功会显示build successful
```
$	ant -f build_zip.xml all
```

接着可以试试运行第一个例子，进入build/bin/mian路径下
```
$	cd build/bin/main
```
然后运行第一个例子
```
$	ant -f runexample.xml -Dnumber=1
```
成功结果如图
![Alt text](http://i1.piimg.com/4851/ca4c87bbf2a8be1a.png)

##  实验心得

本次实验了解了两个新的工具，Markdown、GIt。其中Markdown与之前接触过的文本编辑的语言和工具相比，真的是有简单轻便，使用方便等突出的特性，学会之后在今后的文档编辑、文案制作等地方可以很好的应用。 而Git的优点也显而易见，在项目管理方面有其他工具不能达到的效果，在今后的学习中尝试应用，争取在将来参与的项目中用到这些优秀的工具。
