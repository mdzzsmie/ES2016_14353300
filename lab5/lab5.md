##ROS安装

@(我的第一个笔记本)[分布式, DOL, Markdown]
 
- **SLAM**
- **ROS** 
- **安装流程** 
- **cartographer**


-------------------

[TOC]

-----------------------
#### SLAM
——simultaneous localization and mapping
- 同时定位与建图；简单来说就是——认路 
-  有的用激光雷达做（比如google的cartographer）；有的人用摄 像头做（单目、双目） 
-   ROS上有许多SLAM算法及对应的数据集可以远观和亵玩

--------------------------
#### ROS
——Robot operation system，
- 一套框架，底层提供硬件驱动，软件层 面支持通用的文件格式。
- 我们主要用它的仿真功能。
- 我们在Ubuntu中安装

--------------------------
#### 安装流程

>Ubuntu install of ROS Jade
>1. Installation
1. Configure your Ubuntu repositories
2. Setup your sources.list
3. Set up your keys
4. Installation
5. Initialize rosdep
6. Environment setup
7. Getting rosinstall

>2. Tutorials
1. Obtain source code of the installed packages


##### 1. Installation
 
###### 1.1 Configure your Ubuntu repositories
* Configure your Ubuntu repositories to allow `restricted`,`universe`, and `multiverse`. You can follow the Ubuntu guide for instructions on doing this. 

###### 1.2 Setup your sources.list

* Setup your computer to accept software from packages.ros.org. ROS Jade ONLY supports Trusty (14.04), Utopic (14.10) and Vivid (15.04) for debian packages. 

	sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'


>[SYSU Mirrors](http://ros.exbot.net/rospackage/ros/ )
>
	sudo sh -c '. /etc/lsb-release && echo "deb http://ros.exbot.net/rospackage/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'


###### 1.3 Set up your keys
	sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116

###### 1.4 Installation

* First, make sure your Debian package index is up-to-date: 
```
	sudo apt-get update
```
* 选择Desktop-Full Install
```
	sudo apt-get install ros-jade-desktop-full
```

* To find available packages, use: 
```
	apt-cache search ros-jade
```
######1.5 Initialize rosdep

* Before you can use ROS, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. 
```
sudo rosdep init
rosdep update
```
###### 1.6 Environment setup
* It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched: 
```
echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
* If you just want to change the environment of your current shell, you can type: 
```
source /opt/ros/jade/setup.bash
```
###### 1.7 Getting rosinstall

- rosinstall is a frequently used command-line tool in ROS that is distributed separately. It enables you to easily download many source trees for ROS packages with one command. 

- To install this tool on Ubuntu, run: 
```
sudo apt-get install python-rosinstall
```

##### 2. Tutorials
###### 2.1 Obtain source code of the installed packages

- If you know the location of the repository of each package, you know you can obtain all the code there. But it's often hard even for experienced developers to reach the correct maintained repository of certain packages. Also, in some situations you just want to get the source of the released, installed version of a package. The methods described here the best for these cases. 
```
$ apt-get source ros-jade-laser-pipeline
```
A drawback might be that you have to specify a single, exact package name (asterisks do not work). 

![Alt text](https://github.com/mdzzsmie/ES2016_14353300/blob/master/lab5/img1.png)

------------------------------
 [Ubuntu 14.04，15.04 参考博客](http://wiki.ros.org/jade/Installation/Ubuntu)`
 
--------------

#### Cartographer安装和2D 3D测试图

——Cartographer是Google开源的一个SLAM算法，基于激光雷达以及 IMU（惯性处理单元）

----------
##### 安装依赖项：  

	sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev

-----------
#####首先安装ceres solver，选择的版本是1.11,
先在home目录下新建文件夹，命名为catkin_ws,打开终端进入(cd)该文件的目录下，执行以下代码：   

	git clone https://github.com/hitcm/ceres-solver-1.11.0.git

执行之后，打开catkin_ws文件夹，可以看到名字为ceres-solver-1.11.0的文件夹，打开并新建文件夹build,在终端进入到该目录下（新建文件夹也可以在终端进行），执行以下指令：  
 
	 cd ceres-solver-1.11.0/build

在该目录下依次执行以下指令：  

	cmake ..
	make
	sudo make install

-----------
##### 接着安装cartographer

在catkin_ws下新建文件夹，我的命名为cart，随后在终端进入该目录下，执行以下指令：  

	git clone https://github.com/hitcm/cartographer.git

同样的，执行结束后可以看到一个cartographer的文件夹，在里面新建build文件夹，并在终端进入该路径下：  
	
	cd cartographer
	mkdir build
	cd build


进入后，依次执行以下指令：  

	cmake .. -G Ninja 
	ninja
	ninja test 
	sudo ninja install
	
-------
##### 安装catkin_ws
Install wstool and rosdep. 

	sudo apt-get update 
	sudo apt-get install -y python-wstool python-rosdep ninja-build

	cd catkin_ws 
	wstool init src
Merge the cartographer_ros.rosinstall file and fetch code for dependencies. 

	wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartographer_ros.rosinstall 
	
	wstool update -t src
Install deb dependencies. 

	rosdep init 
	rosdep update 
	rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y

------
#####安装cartographer_ros
进入catkin_ws的src目录下，执行以下指令： 
 
	git clone https://github.com/hitcm/cartographer_ros.git
catkin_ws下执行指令：

	catkin_make

------
##### 跑2d图测试
数据下载测试，我的方法是用迅雷下载到本地再复制到Ubuntu：
[windows下载地址](https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag)

下载完成并复制到Ubuntu后，在终端执行指令：

	roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
执行指令后如果没有错误提示，就可以看到下面的正确图示。

![Alt text](https://github.com/mdzzsmie/ES2016_14353300/blob/master/lab5/img2.jpg)
