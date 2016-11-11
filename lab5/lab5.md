##ROS��װ

@(�ҵĵ�һ���ʼǱ�)[�ֲ�ʽ, DOL, Markdown]
 
- **SLAM**
- **ROS** 
- **��װ����** 
- **cartographer**


-------------------

[TOC]

-----------------------
#### SLAM
����simultaneous localization and mapping
- ͬʱ��λ�뽨ͼ������˵���ǡ�����· 
-  �е��ü����״���������google��cartographer�����е������� ��ͷ������Ŀ��˫Ŀ�� 
-   ROS�������SLAM�㷨����Ӧ�����ݼ�����Զ�ۺ�����

--------------------------
#### ROS
����Robot operation system��
- һ�׿�ܣ��ײ��ṩӲ������������� ��֧��ͨ�õ��ļ���ʽ��
- ������Ҫ�����ķ��湦�ܡ�
- ������Ubuntu�а�װ

--------------------------
#### ��װ����

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
* ѡ��Desktop-Full Install
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

------------------------------
 [Ubuntu 14.04��15.04 �ο�����](http://wiki.ros.org/jade/Installation/Ubuntu)`
 
--------------

#### Cartographer��װ��2D 3D����ͼ

����Cartographer��Google��Դ��һ��SLAM�㷨�����ڼ����״��Լ� IMU�����Դ���Ԫ��

----------
##### ��װ�����  

	sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev

-----------
#####���Ȱ�װceres solver��ѡ��İ汾��1.11,
����homeĿ¼���½��ļ��У�����Ϊcatkin_ws,���ն˽���(cd)���ļ���Ŀ¼�£�ִ�����´��룺   

	git clone https://github.com/hitcm/ceres-solver-1.11.0.git

ִ��֮�󣬴�catkin_ws�ļ��У����Կ�������Ϊceres-solver-1.11.0���ļ��У��򿪲��½��ļ���build,���ն˽��뵽��Ŀ¼�£��½��ļ���Ҳ�������ն˽��У���ִ������ָ�  
 
	 cd ceres-solver-1.11.0/build

�ڸ�Ŀ¼������ִ������ָ�  

	cmake ..
	make
	sudo make install

-----------
##### ���Ű�װcartographer

��catkin_ws���½��ļ��У��ҵ�����Ϊcart��������ն˽����Ŀ¼�£�ִ������ָ�  

	git clone https://github.com/hitcm/cartographer.git

ͬ���ģ�ִ�н�������Կ���һ��cartographer���ļ��У��������½�build�ļ��У������ն˽����·���£�  
	
	cd cartographer
	mkdir build
	cd build


���������ִ������ָ�  

	cmake .. -G Ninja 
	ninja
	ninja test 
	sudo ninja install
	
-------
##### ��װcatkin_ws
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
#####��װcartographer_ros
����catkin_ws��srcĿ¼�£�ִ������ָ� 
 
	git clone https://github.com/hitcm/cartographer_ros.git
catkin_ws��ִ��ָ�

	catkin_make

------
##### ��2dͼ����
�������ز��ԣ��ҵķ�������Ѹ�����ص������ٸ��Ƶ�Ubuntu��
[windows���ص�ַ](https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag)

������ɲ����Ƶ�Ubuntu�����ն�ִ��ָ�

	roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
ִ��ָ������û�д�����ʾ���Ϳ��Կ����������ȷͼʾ��


